### Regex Cheat Sheet
| Regex Statement                                      | Explanation                                                                     |
| ---------------------------------------------------- | ------------------------------------------------------------------------------- |
| /cat/g                                               |                                                                                 |
| /e+/                                                 | matching multiple e's                                                           |
| /ea?/                                                | a is optional                                                                   |
| /re*/                                                | would optionally match e plus more e's such as "reee"                           |
| /.at/                                                | matches with any letter (fat, cat, sat, etc.)                                   |
| \\                                                   | cancel special characters for matching                                          |
| \\w                                                  | match any word letter                                                           |
| \\W                                                  | match any non-word letter                                                       |
| \\s                                                  | match any white space                                                           |
| \\S                                                  | many any non-white space                                                        |
| \\w{4}                                               | 4 letters in a row                                                              |
| \\w{4,}                                              | 4 letters or more                                                               |
| \\w{4,5}                                             | 4-5 letter words                                                                |
| \[fc]at                                              | match anything one in brackets                                                  |
| \[a-z]at                                             | match anything between a-z                                                      |
| \[a-zA-Z]at                                          | makes it case insensitive                                                       |
| \[0-9]at                                             | match anything between 0-9                                                      |
| (t\|T)he                                             | Check for one or the other then the rest                                        |
| (t\|e\|r){2,3}                                       | match 2 or 3 of the tokens                                                      |
| (re){2,3}                                            | would pick up "rere" or "rerere"                                                |
| \^x                                                  | match beginning of the line                                                     |
| \.$                                                  | match the end of statement (street. matches the period)                         |
| (?<=\[tT]he).                                        | (positive look behind) preceded by statement, any letter with "The" before it   |
| (?<!\[tT]he).                                        | (negative look behind) anything that doesn't have the before it, everything but |
| .(?=\at)                                             | (positive look ahead) anything followed by at                                   |
| .(?!\at)                                             | (negative look ahead) anything not followed by at                               |
| \\d{10}                                              | 10 digits in a row                                                              |
| \\d{3}-?\\d{3}-?\\d{4} \                             | 3 digits, optional -, three digits, optional dash, 4 final digits               |
| \\d{3}\[ -]?\\d{3}\[ -]?\\d{4} \                     | same thing but allowing spaces or dashes                                        |
| \\(d{3})\[ -]?(\\d{3})\[ -]?(\\d{4}) \               | now captures numbers to use later in variables                                  |
| \?<\areacode>\(d{3}) no slash                                 | ?<\x>name captured group in variable                                            |
| \\(?(\d{3})\\)?[ -]?(\\d{3})\[ -]?(\\d{4})           | added parenthesis for first ones                                                |
| (\+1[ -])?\(?(\d{3})\)?[ -]?\\(d{3})\[ -]?\\(d{4}) \ | added +1 optional at the beginning                                              |
| (?:xxxxxx)                                           | prevents group from being captured                                              |

(?:\+1[ -])?\(?(\d{3})\)?[ -]?(\d{3})[ -]?(\d{4})

9039208729
903 920 8729
903-920-8729
(903) 920-8729
+1 (903) 920-8729

https://github.com/RocktimRajkumar/regex101-quiz-solution
https://regex101.com/

https://regexone.com/
https://regexcrossword.com/


