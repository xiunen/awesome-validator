# awesome-validator
An awesome form validator

## Home to use

```javascript
// via import
import Validator, {rules} from 'awesome-validator';
//via require
const {default:Validator, rules} = require('awesome-validator');


const form = {username:"test", password:'', retype: 'test'};
const validationConfig = {
  username: [
    {
      ...rules.MIN_LENGTH,
      value: 6,
      message: 'invalid, lenth less than {{number}} charactors'
    }
  ],
  password: [
    rules.REQUIRED,
    {
      ...rules.LENGTH_BETWEEN,
      value: [6, 20],
    }
  ],
  retype:[
    rules.REQUIRED,
    {
      ...rules.FUNCTION,
      value:(value, rule, form)=>{
        return value === form.password
      },
      message: 'retype error'
    }
  ]
}

const {result, messages} = Validator.validate(form, validationConfig);
//  result => false
//  messages => {
//    username: ['invalid, lenth less than 6 charactors'],
//    password: ['required', length btween 6 to 20 required],
//    retype: ['retype error']
//  }

```

## API

### **Validator**, validator instance

### **Validator.validate(form, rules)**, execute validate
**return** {Object}, like:
```javascript
{result: true|false, messages: {
  usename: ['some message']
}}
```
**form**, {Object}

**rules**, {Object}, format like:
```javascript
 {
  username: [
     rule
  ]
 }
```
**rule** format like
```javascript
{
  type: 'min',
  value: 100,
  message:'must greater than {{number}}',
}
```

### **Validator.setMessages(messages)**, override validate message.

**default**, 
```javascript
  {
    required: 'required',
    minlength: 'more than {{number}} chars required',
    maxlength: 'less than {{number}} chars required',
    length_btween: 'length btween {{min}} to {{max}} required',
    max: 'less than {{number}} required',
    min: 'more than {{number}} required',
    number: 'number required',
    number_btween: 'number btween {{min}} to {{max}} required',
    array: 'array required',
    pattern: 'format not matched'
  }
```
`{{number}}`, `{{min}}`, `{{max}}` will be replaced when output message, when `rule.value` is array, the value will replace `{{min}}` and `{{max}}`, otherwise, `{{number}}`

### rules

| type  |  default value |  destructuring from  rule |  description |
|---|---|---|---|
|  required |   |  REQUIRED | string or number required  |
|minlength   |  0 |  MIN_LENGTH | string, number or array min length  |
|  maxlength |  0   | MAX_LENGTH  | string, number or array max length|
|  length_btween |[0, false]|  LENGTH_BWTWEEN |    string, number or array length between range |
|  max |  false | MAX | max number  |
|  min |  false |  MIN | min number  |
|  number |   |  NUMBER | is number?  |
|  number_btween |[false, false] | NUMBER_BETWEEN  |   number between range  |
|  array |   |  ARRAY |  is array?  |
| pattern  | false  |  PATTERN | pattern matched  |
| function  |   ()=>true   |  FUNCTION | user defined validate function|

