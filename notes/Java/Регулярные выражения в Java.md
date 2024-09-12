---
tags: regex
---
`\`  -  экранирование символов  `("\\.")` - "."
                                                                                  Пример
`\W`   - знаки препинания, символы не являющиеся словами `(\\w)`  
`\w` - символы слов [a-zA-Z_0-9], кроме `\\W` 
`\d` - цифры [0-9]
`.` - любой знак
`?` -  повторяется 1 или 0 раз                          `("-?")` "-" знак минус повторяется 0 или 1 раз
`*` -  повторяется 0 или несколько раз         `("-*")`  знак * повторяется 1 или несколько раз
`+` -  повторяется 1 или несколько раз           `("f\\w+")` "f" с несколькими буквами
`|` -  или                                                               `("the | you")`  the или you
	
**Boundary matchers**
`^`   - начало строки 
`$`   - конец строки    `("\\.$")` - строка оканчивается на "."
`\b`  A word boundary: `(?:(?<=\w)(?=\W)|(?<=\W)(?=\w))` (the location where a non-word character abuts a word character)
`\b{g}`A Unicode extended grapheme cluster boundary
`\B` A non-word boundary: `[^\b]`
`\A`  The beginning of the input
`\G' The end of the previous match
`\Z`The end of the input but for the final [terminator](#lt), if any
`\z`The end of the input

`\p{Lower}`A lower-case alphabetic character: `[a-z]`
`\p{Upper}`An upper-case alphabetic character:`[A-Z]`
`\p{ASCII}`All ASCII:`[\x00-\x7F]`
`\p{Alpha}` An alphabetic character:`[\p{Lower}\p{Upper}]`
`\p{Digit}`A decimal digit: `[0-9]`
`\p{Alnum}`An alphanumeric character:`[\p{Alpha}\p{Digit}]`
`\p{Punct}`Punctuation: One of ``!"#$%&'()*+,-./:;<=>?@[\]^_`{|}~``
`\p{Graph}`A visible character: `[\p{Alnum}\p{Punct}]`
`\p{Print}`A printable character: `[\p{Graph}\x20]`
`\p{Blank}`A space or a tab: `[ \t]`
`\p{Cntrl}`A control character: `[\x00-\x1F\x7F]`
`\p{XDigit}`A hexadecimal digit: `[0-9a-fA-F]`
`\p{Space}` A whitespace character: `[ \t\n\x0B\f\r]`