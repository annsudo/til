## EXPRESSION INTERPOLATION
### Single quotes (‘ ’) and Double quotes(“ ”)
When we have to call some variable or when we have to embed any character inside our string we use concatenation + sign. Example:

`var name = 'Geeks';
'Hello ' +name+ '! Welcome.`


### Backticks aka template string (``)
- Allows to use expression inside (“) character, the expression should be wrapped in ${our_expression}. Example:

 `var name = 'Geeks';
 `\``Hello ${name}! Welcome.`\`;

- Template literals still allow you to concatenate your string

`var name = 'Geeks';
`\``Hello `\``+name+`\``! Welcome.`\`;

## MULTILINE STRINGS
### Single quotes (‘ ’) and Double quotes(“ ”)
It’s tiring to use \n character every time when we want to move our string in next line. Example:

`console.log('Its a Line: 1\n' + 'Its a Line: 2\n'+ 'Its a Line: 3');`

### Backticks aka template string (\`\`)
You can use newline without further using any character or concatenation, here is an example:

`console.log(`\``Its a Line: 1
It's a Line: 2
It's a Line: 3`\``);`

## ESCAPING CHARACTER
There is not much difference in escaping character in single, double quotes and backtick string. We can’t use a single quote inside of single quote and double quote inside a double quote, similarly, with backticks, we can’t use backticks within backticks. So to do that we use escape character.

### Single quotes (‘ ’) and Double quotes(“ ”)
we can use single and double quote inside backticks.

`'It\'s a single quote example'  -->> "It´s a single quote example"`

` "It\'s a \"double quote\" example"  -->> "It´s a "double quote" example"`
### Backticks aka template string (\`\`)

\``Its's a \`\``backticks code\`\`` example`\``  --->  Its's a `\``backticks code`\`` example`
