## 概要

https://leetcode.com/problems/unique-email-addresses/description/

## Intuition

@で区切って、左の要素から不要な文字を消して、localと@とdomainをくっつけてanser配列にpush。

重複を排除するため、配列はset関数で作る必要がある。

answer配列の長さが答えになる。

## Complexity

O(n * m)

- O(n) is for iterating through all the emails.
- O(m)O(m) is for processing each email, as described above.

## Code

C++のsplitとjoinのロジックが複雑だったから今回はpythonにした。

```jsx
class Solution(object):
    def numUniqueEmails(self, emails):
        """
        :type emails: List[str]
        :rtype: int
        """
        unique_set = set()
        for email in emails:
            local, domain = email.split("@")
            local = local.split("+")[0]
            local = local.replace(".", "")
            unique_set.add(local + "@" + domain)
        return len(unique_set)
        
```
