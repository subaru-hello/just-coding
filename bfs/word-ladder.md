## 概要

https://leetcode.com/problems/word-ladder/description/

> A **transformation sequence** from word `beginWord` to word `endWord` using a dictionary `wordList` is a sequence of words `beginWord -> s1 -> s2 -> ... -> sk` such that:
> 
> - Every adjacent pair of words differs by a single letter.
> - Every `si` for `1 <= i <= k` is in `wordList`. Note that `beginWord` does not need to be in `wordList`.
> - `sk == endWord`
> 
> Given two words, `beginWord` and `endWord`, and a dictionary `wordList`, return *the **number of words** in the **shortest transformation sequence** from* `beginWord` *to* `endWord`*, or* `0` *if no such sequence exists.*
> 

## Intuition

beginWordとendWordの近い単語を検索していた

s-t最短距離を見つける

それぞれのnode(単語)は2文字以上。

文字間で類似度を測るために中間マップを用意

## Code

66ms

```cpp
#include <iostream>
#include <vector>
#include <unordered_set>
#include <unordered_map>
#include <queue>

using namespace std;

class Solution {
public:
    int ladderLength(string beginWord, string endWord, vector<string>& wordList) {
        unordered_set<string> wordSet(wordList.begin(), wordList.end());
        
        // If endWord is not in the wordList, there is no valid transformation
        if (wordSet.find(endWord) == wordSet.end()) return 0;

        // Initialize a queue for BFS
        queue<pair<string, int>> q;
        q.push({beginWord, 1});  // Start with beginWord and initial length of 1
        
        // Perform BFS
        while (!q.empty()) {
            auto [currentWord, level] = q.front();
            q.pop();
            
            // Try all possible one-letter transformations of the current word
            for (int i = 0; i < currentWord.size(); i++) {
                string tempWord = currentWord;
                
                // Change one letter at a time
                for (char c = 'a'; c <= 'z'; c++) {
                    tempWord[i] = c; // hot→aot,bot,cot,dot...
                    
                    // If the transformed word is endWord, return the transformation length
                    if (tempWord == endWord) {
                        return level + 1;
                    }
                    
                    // If the transformed word is in the wordSet
                    if (wordSet.find(tempWord) != wordSet.end()) {
                        q.push({tempWord, level + 1});  // Push the new word with incremented level
                        wordSet.erase(tempWord);  // Remove the word from the set to mark it as visited
                    }
                }
            }
        }
        
        return 0;  // If no transformation is possible
    }
};

int main() {
    Solution solution;
    string beginWord = "hit";
    string endWord = "cog";
    vector<string> wordList = {"hot", "dot", "dog", "lot", "log", "cog"};

    cout << "Length of shortest transformation sequence: " << solution.ladderLength(beginWord, endWord, wordList) << endl;
    // Output: 5 ("hit" -> "hot" -> "dot" -> "dog" -> "cog")

    return 0;
}

```

20ms

```cpp
#include <unordered_set>
#include <unordered_map>
#include <vector>
#include <queue>
#include <string>

class Solution {
public:
    // The main function that starts the bidirectional BFS
    int ladderLength(std::string beginWord, std::string endWord, std::vector<std::string>& wordList) {
        // Initialize a set with the words for fast lookup
        std::unordered_set<std::string> words(wordList.begin(), wordList.end());
        // If the end word is not in the set, no transformation sequence exists
        if (!words.count(endWord)) return 0;

        // Two queues for the bidirectional BFS
        std::queue<std::string> queueBegin{{beginWord}};
        std::queue<std::string> queueEnd{{endWord}};

        // Mapping from word to its number of steps from the begin word or end word
        std::unordered_map<std::string, int> stepsFromBegin;
        std::unordered_map<std::string, int> stepsFromEnd;

        // Initial steps counts for beginWord and endWord
        stepsFromBegin[beginWord] = 1;
        stepsFromEnd[endWord] = 1;

        // Bidirectional BFS
        while (!queueBegin.empty() && !queueEnd.empty()) {
            // Choose the direction with the smaller frontier for extension
            int result = queueBegin.size() <= queueEnd.size()
                ? extendQueue(stepsFromBegin, stepsFromEnd, queueBegin, words)
                : extendQueue(stepsFromEnd, stepsFromBegin, queueEnd, words);

            if (result != -1) return result; // If paths meet, return the result
        }
        return 0; // If no path is found, return 0
    }

    // Extend one level of BFS, updating the queue and steps count
    int extendQueue(std::unordered_map<std::string, int>& currentSteps, 
                    std::unordered_map<std::string, int>& oppositeSteps, 
                    std::queue<std::string>& currentQueue, 
                    std::unordered_set<std::string>& words) {
      
        for (int i = currentQueue.size(); i > 0; --i) {
            std::string word = currentQueue.front();
            int step = currentSteps[word];
            currentQueue.pop();

            // Try to transform each position in the word
            for (int j = 0; j < word.size(); ++j) {
                char originalChar = word[j];

                // Replace the current character with all possible characters
                for (char c = 'a'; c <= 'z'; ++c) {
                    word[j] = c;
                    // Continue if the transformed word is the same or not in the word set
                    if (!words.count(word) || currentSteps.count(word)) continue;
                    // If the transformed word is in the opposite steps, a connection is found
                    if (oppositeSteps.count(word)) return step + oppositeSteps[word];

                    // Otherwise, add the transformed word into the current steps and queue
                    currentSteps[word] = step + 1;
                    currentQueue.push(word);
                }

                // Change back to the original character before trying the next position
                word[j] = originalChar;
            }
        }
        return -1;
    }
};
```
