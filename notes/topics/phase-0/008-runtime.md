# Topic 8 — Runtime kya hota hai?

## Aaj ka exact objective

Aaj **runtime** ka mental model samajhna hai: source instructions ko execute karne
ke liye kaunsa environment chahiye aur woh environment program ko kya capabilities
deta hai. Source code aur active running process ka exact difference Topic 9 mein
aayega; aaj koi JavaScript file execute nahi hogi.

## Problem: code khud se nahi chalta

Imagine file mein likha hai:

```js
2 + 3
```

Yeh instruction/value expression file mein text ke form mein ho sakta hai. Computer
ko ise JavaScript ke rules ke according samajhne aur execute karne ke liye compatible
execution environment chahiye.

## Runtime ki simple definition

**Runtime woh execution environment hota hai jisme program run hota hai. Woh code ko
execute karne aur program ko required built-in capabilities/resources access karne
ki facility deta hai.**

Simple version:

> Programming language batati hai code likhne ke rules; runtime us code ko chalne ka
> environment deta hai.

## Workshop analogy

```text
Written instructions = source code
Instruction language = JavaScript rules/syntax
Equipped workshop    = runtime
Available tools      = runtime-provided capabilities
Performed work       = program behaviour
```

Instructions mein "wood cut karo" likha ho, lekin workshop mein cutting tool hi na
ho, to instruction perform nahi hogi. Waise hi ek environment ki capability doosre
runtime mein available hona guaranteed nahi.

## Runtime generally kya provide karta hai?

Runtime/environment commonly:

- code ko parse/understand karne ka mechanism;
- instructions execute karne ka engine;
- memory use karne ki facility;
- built-in functions/objects;
- errors produce/report karne ka mechanism;
- timers, files, network ya UI jaise environment-specific capabilities;
- operating system/resources ke saath controlled interaction.

Har runtime exactly same capabilities nahi deta.

## JavaScript language aur runtime

JavaScript ek programming language hai. Woh syntax aur behaviour rules define karti
hai, jaise variables, functions, objects aur expressions.

JavaScript runtime us language mein written code ko execute karta aur environment-
specific capabilities deta hai.

```text
JavaScript language = code likhne/behave karne ke rules
JavaScript runtime  = code execute hone ka environment
```

Language aur runtime related hain, identical nahi.

## Browser runtime

Browser JavaScript ko execute karne ka runtime environment provide karta hai.
Browser ka main context web pages hai, isliye woh capabilities de sakta hai:

- page ke elements interact/change karna;
- click/typing events handle karna;
- browser storage use karna;
- server communication initiate karna;
- timers use karna.

Exact browser APIs aur implementation abhi scope mein nahi hain.

## Node.js runtime

Node.js JavaScript ko browser ke bahar, commonly server-side ya development tools ke
context mein execute karne wala runtime hai.

Node.js capabilities ke examples:

- files aur paths ke saath kaam;
- server program chalana;
- operating-system/process information access;
- command-line tools banana;
- packages/modules use karna.

TaskForge backend eventually Node.js runtime mein execute hoga. Node install/version
Phase 1 mein aur detailed foundation Phase 4 mein aayegi.

## Browser aur Node.js mein same JavaScript?

Core JavaScript concepts same ho sakte hain:

```js
const taskTitle = "Learn runtime";
```

Lekin environment capabilities different ho sakti hain:

- Browser page/UI ko access kar sakta hai.
- Node.js filesystem/server-side capabilities de sakta hai.
- Browser-specific object Node.js mein available hona guaranteed nahi.
- Node-specific capability browser mein available hona guaranteed nahi.

Isliye "JavaScript valid hai" aur "is runtime mein available hai" do different
questions hain.

## TaskForge mein runtimes

Future conceptual picture:

```text
Browser runtime
  -> TaskForge frontend JavaScript execute kar sakta hai
  -> client request bhej sakta hai

Node.js runtime
  -> TaskForge backend JavaScript execute karega
  -> server behaviour, rules aur database coordination sambhalega
```

Frontend optional/later hai; current focus backend Node.js learning par rahega.

## Runtime aur operating system

Operating system—jaise Windows—computer resources manage karta hai. Runtime OS ke
upar run karke application code ko useful programming environment deta hai.

Conceptual layers:

```text
Application code
      |
    Runtime
      |
Operating system
      |
   Hardware
```

Yeh simplified model hai; exact internals later topics mein relevant hone par aayenge.

## Runtime aur editor

VS Code editor hai: usmein code likha/read kiya ja sakta hai. VS Code mein file open
hona automatically us application code ko execute nahi karta.

```text
VS Code = writing/editing tool
Node.js = JavaScript execution runtime
```

Editor ke andar terminal/debugger integration ho sakti hai, lekin roles different
hain.

## Runtime aur framework

- Node.js runtime hai.
- Express future backend framework/library hoga jo Node.js runtime ke andar use hoga.

Express JavaScript ko independently execute karne wala runtime nahi. Pehle compatible
runtime chahiye.

## Runtime error ka basic meaning

Runtime error woh problem hai jo code execute karte time appear hoti hai.

Possible conceptual reasons:

- unavailable variable/capability use hui;
- invalid operation execute hui;
- required file/resource nahi mila;
- permission unavailable;
- unexpected data mila.

Syntax errors aur detailed Node error stacks later topics mein properly cover honge.

## Same code, different runtime outcome

Ek code environment-specific capability maang sakta hai:

```text
Code asks: browser page ka button find karo
Browser runtime: page capability available ho sakti hai
Node.js runtime: browser page normally available nahi
```

Is case mein programming language same hai, lekin execution environment different
hai.

## Runtime version kyun matter karti hai?

Different runtime versions:

- different language features support kar sakti hain;
- built-in APIs ka behaviour/capability change kar sakti hain;
- security fixes la sakti hain;
- package compatibility affect kar sakti hain.

Isliye TaskForge environment setup mein Node version verify aur document karenge.
Exact verification Topic 28 mein hogi.

## Common misconceptions

1. **"JavaScript aur Node.js same hain."**  
   JavaScript language hai; Node.js us language ka runtime hai.

2. **"VS Code runtime hai."**  
   VS Code primarily editor/development tool hai.

3. **"Browser mein chalne wala har code Node.js mein same chalega."**  
   Core language same ho sakti hai, environment-specific capabilities different hain.

4. **"Runtime aur framework same hain."**  
   Node.js runtime hai; Express runtime ke andar use hone wala framework/library hai.

5. **"Code file save karte hi application run ho gayi."**  
   Save aur execution separate events hain. Exact distinction Topic 9 mein aayega.

6. **"Runtime sirf code translate karta hai."**  
   Woh memory, built-ins, errors aur environment/resource access bhi provide karta
   hai.

## Verification strategy

Topic understood hai agar learner:

1. runtime ko apne words mein define kar sake;
2. language aur runtime ka difference explain kar sake;
3. browser aur Node.js runtime ka ek-ek capability example de sake;
4. VS Code, Node.js aur Express ki roles separate kar sake;
5. explain kar sake ki same JavaScript code different runtimes mein different result
   kyun de sakta hai;
6. TaskForge backend ka future runtime identify kar sake.

## Quick self-check

1. Runtime kya hota hai?
2. JavaScript language hai ya runtime?
3. Node.js language hai ya runtime?
4. Browser aur Node.js ki ek-ek environment-specific capability batao.
5. VS Code code edit karta hai ya JavaScript runtime provide karta hai?
6. TaskForge backend future mein kis runtime mein chalega?

## Practice exercise

Neeche ka table apne words mein fill karo:

```text
Item: JavaScript
Role:

Item: Node.js
Role:

Item: Browser
Role as runtime:

Item: VS Code
Role:

Item: Express
Role:

One browser-only capability example:
One Node-side capability example:
```

Pehle khud attempt karo, phir sample dekho.

<details>
<summary>Answer-after-attempt</summary>

```text
JavaScript: Programming language
Node.js: Browser ke bahar JavaScript execute karne wala runtime
Browser: Web-page JavaScript ka runtime/environment
VS Code: Code editor/development tool
Express: Node.js mein use hone wala backend framework/library
Browser capability: Page element change karna
Node-side capability: Filesystem/server-side operation
```

</details>

## Interview question with Hinglish answer

**Question:** Runtime kya hota hai? JavaScript aur Node.js ke context mein samjhao.

**Answer:** Runtime execution environment hota hai jo program code ko execute karta
aur memory, built-in APIs aur environment resources ki capabilities deta hai.
JavaScript programming language hai, jabki Node.js JavaScript ko browser ke bahar
execute karne wala runtime hai. Browser bhi JavaScript runtime provide karta hai,
lekin browser aur Node.js ki environment-specific capabilities different hoti hain.

## Easy-English minimum interview answer

**A runtime is the environment in which program code executes. It provides the
engine, memory, built-in APIs, and access to environment resources. JavaScript is a
programming language, while Node.js is a runtime that executes JavaScript outside the
browser.**

### Even shorter version

**A runtime is the environment that executes code and provides the capabilities it
needs to run.**

## Topic boundary

Topic 8 mein runtime ka foundational mental model complete hua. **Source code aur
running process ka difference** Topic 9 ko iske baad separately complete kiya gaya.
