In PEG.js, this source code:

```
{

const TAB = "  ";
const EOL = "\n";

function decodeGatewayPatternMethods(calls, sentence) {
  let o = "";
  calls.forEach(call => {
    if(call.async) {
      o += "  async " + call.text + "(...args) {\n"
        +  "    // @TODO...\n"
        +  "  }\n";
    } else {
      o += "  " + call.text + "(...args) {\n"
        +  "    // @TODO...\n"
        +  "  }\n";
    }
  });
  return o;
}

function decodeGatewayPatternCalls(calls, sentence) {
  let o = "";
  calls.forEach(call => {
    if(call.async) {
      o += "      await " + call.text + "(...args);\n";
    } else {
      o += "      " + call.text + "(...args);\n";
    }
    if(sentence.interpolation) {
      o += "      " + sentence.interpolation + "\n";
    }
  });
  return o;
}

function decodeGatewayPattern(s) {
  let o = ""
        + "  " + s.method + "(...args) {\n"
        + "    try {\n"
        + "" + decodeGatewayPatternCalls(s.calls, s) + ""
        + "    } catch(error) {\n"
        + "      this.onHandleError(error);\n"
        + "    }\n"
        + "  }\n"
        + "  \n"
        + "" + decodeGatewayPatternMethods(s.calls, s) + "\n"
        + "\n"
        + "";
  return o;
};

function decode(s) {
  let o = "class _ {\n";
  switch(s.type) {
    case "gateway pattern":
    o += decodeGatewayPattern(s);
    break;
  }
  o += "\n}";
  return o;
}

}

language = __* s:full_sentence __* {return decode(s)}
full_sentence = s:sentence EOS {return s}
sentence = gateway_pattern_sentence
gateway_pattern_sentence = "gateway:" __ __ c:gateway_pattern_contents? {return {type:"gateway pattern", ...c}}
gateway_pattern_contents = m:method_definition i:calls_interpolation? c:method_calls? {return {method:m,interpolation:i,calls:c}}
method_definition = name {return text().replace(/\n+/g,"")}
calls_interpolation = __ "--" _? t:NEOL {return t}
method_calls = method_call+
method_call = __ a:("+"/"~") _? t:NEOL _*  {return {async: a==="+" ? false : true, text: t}}
NEOL = (!(__/EOF).)+ {return text()}
EOS = __/EOF
_ = [\t ]
__ = "\n" / "\r\n"
name = [A-Za-z_$] [A-Za-z0-9_$]* {return text()}
EOF = !.
```

...makes this sample (that contains an example of all the syntax):

```
gateway:

registerCallback
-- if(parameters.exit) {return parameters.exit}
+ onPrepareSelectUserQuery
+ onPrepareSelect
+ onPrepareFrom
+ onPrepareFields
+ onPrepareJoinMembership
+ onPrepareJoinRole
+ onPrepareJoinCommunity
+ onPrepareJoinPermission
+ onPrepareWhereCredentials
~ onSelectForUser
+ onCheckForUser
+ onPrepareInsertUserQuery
+ onPrepareInsert
+ onPrepareInto
+ onPrepareValues
+ onInsertForUser
+ onResponse
```

...output this source:

```
class _ {
  registerCallback(...args) {
    try {
      onPrepareSelectUserQuery(...args);
      if(parameters.exit) {return parameters.exit}
      onPrepareSelect(...args);
      if(parameters.exit) {return parameters.exit}
      onPrepareFrom(...args);
      if(parameters.exit) {return parameters.exit}
      onPrepareFields(...args);
      if(parameters.exit) {return parameters.exit}
      onPrepareJoinMembership(...args);
      if(parameters.exit) {return parameters.exit}
      onPrepareJoinRole(...args);
      if(parameters.exit) {return parameters.exit}
      onPrepareJoinCommunity(...args);
      if(parameters.exit) {return parameters.exit}
      onPrepareJoinPermission(...args);
      if(parameters.exit) {return parameters.exit}
      onPrepareWhereCredentials(...args);
      if(parameters.exit) {return parameters.exit}
      await onSelectForUser(...args);
      if(parameters.exit) {return parameters.exit}
      onCheckForUser(...args);
      if(parameters.exit) {return parameters.exit}
      onPrepareInsertUserQuery(...args);
      if(parameters.exit) {return parameters.exit}
      onPrepareInsert(...args);
      if(parameters.exit) {return parameters.exit}
      onPrepareInto(...args);
      if(parameters.exit) {return parameters.exit}
      onPrepareValues(...args);
      if(parameters.exit) {return parameters.exit}
      onInsertForUser(...args);
      if(parameters.exit) {return parameters.exit}
      onResponse(...args);
      if(parameters.exit) {return parameters.exit}
    } catch(error) {
      this.onHandleError(error);
    }
  }
  
  onPrepareSelectUserQuery(...args) {
    // @TODO...
  }
  onPrepareSelect(...args) {
    // @TODO...
  }
  onPrepareFrom(...args) {
    // @TODO...
  }
  onPrepareFields(...args) {
    // @TODO...
  }
  onPrepareJoinMembership(...args) {
    // @TODO...
  }
  onPrepareJoinRole(...args) {
    // @TODO...
  }
  onPrepareJoinCommunity(...args) {
    // @TODO...
  }
  onPrepareJoinPermission(...args) {
    // @TODO...
  }
  onPrepareWhereCredentials(...args) {
    // @TODO...
  }
  async onSelectForUser(...args) {
    // @TODO...
  }
  onCheckForUser(...args) {
    // @TODO...
  }
  onPrepareInsertUserQuery(...args) {
    // @TODO...
  }
  onPrepareInsert(...args) {
    // @TODO...
  }
  onPrepareInto(...args) {
    // @TODO...
  }
  onPrepareValues(...args) {
    // @TODO...
  }
  onInsertForUser(...args) {
    // @TODO...
  }
  onResponse(...args) {
    // @TODO...
  }

}
```

"# design-pattern-gen-language" 
