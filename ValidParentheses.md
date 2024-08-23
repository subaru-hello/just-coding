## Problem Statement

https://leetcode.com/problems/valid-parentheses/description/

## Intuition

stackスペースを作成して、openが来たらpush、closeが来たらstackを見て対のopenがあればpop、なければfalseって感じで進めるのか。

受け取る、openは優先して入れる、closeは中身を確認してから入れる、最後にstackがemptyか確認する、みたいな感じ。

## Solution
