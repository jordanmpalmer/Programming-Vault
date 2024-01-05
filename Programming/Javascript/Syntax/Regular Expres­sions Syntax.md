| Syntax    | Description             |
| --------- | ----------------------- |
| ^         | Start of string         |
| $         | End of string           |
| .         | Any single character    |
| (a\|b)    | a or b                  |
| (...)     | Group section           |
| [abc]     | In range (a, b or c)    |
| [\^abc]   | Not in range            |
| \s        | White space             |
| a?        | Zero or one of a        |
| a*        | Zero or more of a       |
| a*?       | Zero or more, ungreedy  |
| a+        | One or more of a        |
| a+?       | One or more, ungreedy   |
| a{3}      | Exactly 3 of a          |
| a{3,}     | 3 or more of a          |
| a{,6}     | Up to 6 of a            |
| a{3,6}    | 3 to 6 of a             |
| a{3,6}?   | 3 to 6 of a, ungreedy   |
| '\\'      | Escape character        |
| [:punct:] | Any punctu­ation symbol |
| [:space:] | Any space character     |
| [:blank:] | Space or tab            |

There's an excellent regular expression tester at: [http:/­/re­gex­pal.com/](http://regexpal.com/)