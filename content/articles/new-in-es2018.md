---
title: "الجديد في إكماسكربت 2016 و 2017 و 2018"
date: 2021-05-29
---

هذه المقالة جزء من مجموعة مقالات تلخص التغييرات المضافة على توصيف إكماسكربت، الذي يعتبر المعيار القياسي الذي تتوافق معه لغة جافاسكربت، منذ عام 2016 وحتى عام 2021. لقراءة الأجزاء الأخرى:

[تعديلات إكماسكربت عام 2019](https://muhammetsait.com/articles/new-in-es2019/)

[تعديلات إكماسكربت عام 2020](https://muhammetsait.com/articles/new-in-es2020/)

[تعديلات إكماسكربت عام 2021](https://muhammetsait.com/articles/new-in-es2021/)


يمكنك متابعة جديد هذه التغييرات عبر مستودع مقترحات إكماسكريبت على جت هب [[1]](https://github.com/tc39/proposals). هناك أيضاً جدول توافقية [[2]](https://kangax.github.io/compat-table/es2016plus/) يبين مدى توافر هذه الميزات على المتصفحات والمنصات المختلفة.


## إضافات ES2016

### # [Array.prototype.includes](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/includes)

تستخدم هذه الدالة لمعرفة إذا كانت المصفوفة تحوي قيمة ما ضمن مدخلاتها، وتعيد القيمة `true` أو `false`.

```javascript
const pets = ['cat', 'dog', 'bat'];

console.log(pets.includes('cat'));
// expected output: true
```

### # [عامل الرفع إلى قوة \*\*](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Exponentiation)

يعطي ناتج رفع العدد الأول إلى قوة العدد الثاني. أي الأساس ** الأس. ناتج العملية يكافئ ناتج استدعاء الدالة `Math.pow`، لكنه يتميز عنها بأنه يدعم الأعداد من نوع BigInt. هذا العامل أيضاً يخضع لتحسينات الأداء بشكل أفضل من `Math.pow` في بعض الحالات مما قد يعطي أداء أعلى نسبياً.

```javascript
console.log(3 ** 4);
// expected output: 81

console.log(10 ** -2);
// expected output: 0.01
```


## إضافات ES2017

### # [Object.values](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/values)

تعيد هذه الدالة مصفوفة تحوي كافة القيم التي يحويها الكائن الممرر لها.

```javascript
const object1 = {
  a: 'somestring',
  b: 42,
  c: false
};

console.log(Object.values(object1));
// expected output: Array ["somestring", 42, false]
```

### # [Object.entries](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/entries)

تعيد هذه الدالة مصفوفة فيها مصفوفات جزئية. كل مصفوفة جزئية تكافئ مدخلة واحدة في الكائن، وتحوي عنصرين هما المفتاح والقيمة لتلك المدخلة.

```javascript
const object1 = {
  a: 'somestring',
  b: 42
};

console.log(JSON.stringify(Object.entries(object1)));
// expected output: [["a","somestring"],["b",42]]
```

هذه الدالة مفيدة عند الرغبة بالتكرار على كل المدخلات في الكائن وتفيذ عملية ما تحتاج الوصول إلى المفتاح والقيمة معاً. قارن بين الأمثلة التالية التي تطبع مفاتيح وقيم مدخلات الكائن `object1`:

```javascript
const object1 = {
  a: 'somestring',
  b: 42,
  c: false
};

// Loop using the old way
var k = Object.keys(object1);
for(i = 0; i < k.length; i++) {
    console.log(k[i], object1[k[i]]);
}

// Modern way
Object.entries(object1).forEach(([k, v]) => { console.log(k, v); });

// Alternative modern way
for (let [key, val] of Object.entries(object1)) {
  console.log(key, val);
}

// expected output:
// a somestring
// b 42
// c false
```

### # [String.prototype.padEnd](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/padEnd) / [String.prototype.padStart](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/padStart)

دوال حشو السلاسل النصية. تستخدم لتمديد طول السلسلة إلى الطول الحدد من المحارف بإضافة فراغات أو بتكرار السلسلة النصية الممرة كمتغير ثان للدالة.

```javascript
'z'.padEnd(5);
// expected output: "z    "

'z'.padEnd(25, '.+.');
// expected output: "z.+..+..+..+..+..+..+..+."
```

_انظر أيضاً `trimStart` و `trimEnd` التي أدرجت في إكماسكربت 2019_

### # [Object.getOwnPropertyDescriptors](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/getOwnPropertyDescriptors)

دالة جديدة متممة لعمل الدالة المفردة [`Object.getOwnPropertyDescriptor`](https://wiki.hsoub.com/JavaScript/Object/getOwnPropertyDescriptor). هذه الدالة تعيد واصفات كافة الخواص المعرفة ضمن الكائن نفسه.

```javascript
const object1 = {
  property1: 42
};

const descriptors1 = Object.getOwnPropertyDescriptors(object1);

console.log(descriptors1);

//expected output:
//  {
//    property1: {
//      value: 42,
//      writable: true,
//      enumerable: true,
//      configurable: true
//    }
//  }

```

### # [الفواصل الإضافية في نهاية بارمترات الدوال عند التعريف أو الاستدعاء](https://developer.mozilla.org/en-us/docs/Web/JavaScript/Reference/Trailing_commas)

الفاصلة الإضافية هي علامة ( `,` ) التي توضع بعد العنصر الأخير في المصفوفة أو الكائن أو مجموعة بارامترات الدالة عند تعريفها أو استدعائها. كتابة الفاصلة الإضافية غير مسموحة في JSON، لكنها كانت مسموحة في تعريف المصفوفات في جافاسكربت منذ بداية عهدها، ثم تم السماح بإضافة الفاصلة النهائية إلى تعريفات الكائنات في ES5 وإلى قائمة بارمترات الدوال في ES2017.

لا يسمح باستخدام الفاصلة الإضافية في تعريف الدالة إذا لم تكن لها أي بارمترات.

```javascript
function add(x, y, ) { // ok
  return x + y;
}

console.log(add(3, 4, ));
// expected output: 7

function test(, ) { // syntax error
}
```

### # [`async / await`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/async_function)

يمكن تعريف الدوال اللامتزامنة باستخدام الكلمة المفتاحية الجديدة: `aync`. ضمن هذه الدوال، يمكنك استخدام الكلمة المفتاحية  `await` لانتظار حسم نتيجة وعد ما (Promise) وتخزينها في متغير بدلاً من استخدام سلاسل طويلة من استدعاءات `then` وكتابة دوال متداخلة. هذه الطريقة تؤدي لكتابة أكواد أنظف، كما تسمح باستخدام بلوكات `try / catch` حول استدعاءات الوعود.

كل الدوال التي تعرف بالكلمة المفتاحية `async` هي كائنات من نوع [`AsyncFunction`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/AsyncFunction). القيمة التي تعيدها الدالة اللامتزامنة هي دوماً من نوع Promise. إذا كانت الدالة تعيد قيمة أخرى فسوف تغلف تلقائياً ضمن Promise.

### # [مشاركة الذاكرة](https://github.com/tc39/ecmascript_sharedmem/blob/master/TUTORIAL.md) و [Atomics](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Atomics)

أضيف كائن [`SharedArrayBuffer`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/SharedArrayBuffer) الذي يسمح بمشاركة الذاكرة بين السكربت الرئيسي وبين عمال الويب (Web Workers) الذين يتخاطب معهم. كما أضيف كائن [`Atomics`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Atomics) الذي يوفر عمليات ذرية (غير قابلة للمقاطعة) لاستخدامها في مزامنة الذاكرة المشتركة بين خيوط المعالجة التي تتعامل معها.

## إضافات ES2018

### # [تعديل على القوالب الحرفية](https://tc39.es/proposal-template-literal-revision/)

القوالب الحرفية Template literals هي سلاسل محرفية خاصة يمكنك فيها طباعة قيم بعض المتغيرات أو تنفيذ عمليات حسابية بهدف طباعة سلسلة منسقة بشكل جميل. يتم تعريف هذه السلاسل باستخدام علامة ( <code>`</code> ) ويمكن كتابتها على عدة سطور.

الجديد في ES2018 هو تعديل يسمح باستخدام سلاسل التهريب غير الصالحة في القوالب الحرفية الموسومة tagged template literals، بدلاً من إعادة خطأ.

```javascript
console.log(`\u0620 or \u{1f602} this is ok.`);
// expected output: "ؠ or 😂 this is ok."

let str = `\unicode ${1} hello ${2}.`;
// Uncaught SyntaxError: Invalid Unicode escape sequence
```

في المثال السابق، التعليمة الأولى عرفنا فيها قالباً نصياً واستخدمنا <code>&lrm;\u</code> لتهريب محرفين من محارف يونيكود، أما التعليمة الثانية فتعطي خطأ، لأن `nicode` ليس مقبولاً كمحرف يونيكود.

لكن هناك حالات تحتاج فيها للتعامل مع سلاسل التهريب غير المسموحة كما في حالة كتابة كود لمعالجة نصوص LaTeX أو ما شابه. في تلك الحالة، يمكنك وسم القالب الحرفي، وسيتم تمرير القالب كما هو دون معالجة إلى دالة الوسم حيث يمكن معالجته. انظر المثال التالي لمزيد من الإيضاح.

```javascript
function latex(str) {
 return { "cooked": str, "raw": str.raw }
}

console.log(latex`\unicode ${1} hello ${2}.`)
// expected output: {
// cooked: [undefined, " hello ", ".", raw: Array(3)]
// raw: ["\unicode ", " hello ", "."]
// }
```

كما ترى، فقد تجاهلت جافاسكريبت الخطأ، ومررت السلسلة الأولى بشكلها الخام إلى دالة الوسم، أما بالنسبة للشكل المطبوخ -الذي يفترض أن تستبدل جافاسكريبت محارف التهريب فيه بقيمها المناسبة- فهو يحوي القيمة `undefined`.

### # [علامة `s` في التعبيرات المنتظمة (`dotAll`)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/RegExp/dotAll) [[1]](https://github.com/tc39/proposal-regexp-dotall-flag)

تمت إضافة وضع جديد للتعبيرات المنتظمة (Regular Expressions)، حيث يتم فيه مطابقة أي محرف مع رمز النقطة: `.` (من هنا سمي وضع `dotAll`). الفكرة أن رمز `.` في جافاسكربت لم يكن يطابق بعض المحارف رغم أنه يفترض أن يطابق "أي شيء". بشكل أخص، رمز `.` لا يطابق بعض محارف المسافات البيضاء (مثل محرف نهاية السطر ونهاية المقطع) في الحالة الافتراضية. كما أن المحارف الفلكية (astral characters) في يونيكود لا تطابق مع رمز `.` إلا باستخدام الرمز `s` لتفعيل نمط `dotAll` مع إضافة الرمز `u` من أجل يونيكود.

### # [تسمية المجموعات في RegExp](https://github.com/tc39/proposal-regexp-named-groups)

في التعبيرات المنتظمة، يمكن تعريف "مجموعات التقاط capture groups" تمثل أجزاءاً محددة من السلسلة المحرفية التي يطابقها التعبير المنتظم.

سابقاً، كانت الطريقة الوحيدة للتعامل مع مجموعات الالتقاط هي باستخدام أرقامها، حيث ترقّم المجموعات تسلسلياً حسب ترتيب ورودها بدءاً من الصفر. المثال التالي فيه تعبير منتظم يطابق التواريخ المكتوبة حسب صيغة ISO المعيارية ويستخدم مجموعات الالتقاط للتعامل مع السنة والشهر واليوم بشكل منفصل:

```javascript
let re = /(\d{4})-(\d{2})-(\d{2})/u;
let result = re.exec('2015-01-02');

// result[0] === '2015-01-03';
// result[1] === '2015';
// result[2] === '01';
// result[3] === '02';
```

لكن مع هذا التغيير أصبح اسناد تسميات للمجموعات ممكناً، مما يسهل التعامل معها، كما أن هذا يحل مشكلة تغير الرقم الدال على مجموعة ما إذا تغير ترتيب تعريف المجموعات في التعبير المنتظم (كأن نستخدم تعبيراً لمطابقة صيغة تاريخ أخرى ترتيب الشهر واليوم والسنة فيها مختلف).

لتسمية مجموعة التقاط ما، نستخدم الصيغة التالية <code>&lrm;(?&lt;name>&lrm; ... )</code>، مع الأخذ بعين الاعتبار أن الاسم يجب أن يحقق شروط تسمية المتغيرات العادية (يتكون من حروف وأرقام و `_` فقط ولا يبدأ برقم):

```javascript
let re = /(?<year>\d{4})-(?<month>\d{2})-(?<day>\d{2})/u;
let result = re.exec('2015-01-02');

// result.groups.year === '2015';
// result.groups.month === '01';
// result.groups.day === '02';
```

يمكن أيضاً استخدم صيغة التفكيك destructring مع هذه الصيغة، كما يلي:

```javascript
let {groups: {year, month, day}} =
    /(?<year>\d{4})-(?<month>\d{2})-(?<day>\d{2})/u
    .exec('2015-01-02');
    
// year = 2015
// month = 01
// day = 02
```

### # [RegExp Lookbehind Assertions](https://github.com/tc39/proposal-regexp-lookbehind)

أضيفت تأكيدات البحث الخلفي في التعبيرات المنتظمة كمتمم لتأكيدات البحث الأمامي الموجودة سابقاً ([Lookahead Assertions](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Regular_Expressions/Assertions)).

تكتب هذه التأكيدات وفق الصيغة التالية <code>&lrm;(?&lt;=y)x</code> للتأكيد الخلفي المثبت (أو الموجب Positive lookbehind assertion)، حيث تتم مطابقة التعبير المنتظم `x` فقط إذا كان مسبوقاً بسلسلة محارف تطابق التعبير `y`. أما التأكيد الخلفي المنفي (أو السالب Negative lookbehind assterion) فيأخذ الصيغة <code>&lrm;(?&lt;!y)x</code> وهو يستخدم لمطابقة التعبير `x` إذا وفقط إذا لم يكن مسبوقاً بسلسلة محارف تطابق التعبير `y`.

مثلاً، لمطابقة سعر منتج ما ضمن نص قد يحوي أرقاماً أخرى لا تمثل أسعاراً، وعلى فرض أن السعر يسبق برمز عملة الدولار `$`، يمكننا استخدام التعليمات التالية:

```javascript
const productDescription = "Just try it and if you don't like it within the next 90 days, we'll refund every penny! Starting from $39.75. Order within the next 4 hours and receive your product by tomorrow.";

const regex = /(?<=\$)\d+\.?\d*/; // Match one or more digits,
                                  // followed by one optional dot `.`,
                                  // followed by zero or more digits,
                                  // with a lookbehind assertions for the `$` symbol
                                    
console.log(productDescription.match(regex));
// expected output: Array ["39.75"]

```

### # [RegExp Unicode Property Escapes](https://github.com/tc39/proposal-regexp-unicode-property-escapes)

هذا التغيير يضيف طريقة جديدة لمطابقة محارف يونيكود في السلاسل المحرفية حسب اسم اللغة أو حسب تصنيف المحارف في معيار يونيكود. هذا التغيير يسمح بمطابقة المحارف التي تنتمي لمجموعة معينة أو لغة ما دون تعريف مجالات يونيكود أو استخدام مكتبات خارجية.

في المثال التالي، لدينا ثلاثة تعابير منتظمة كلها تطابق مجموعات الحروف العربية الواردة في السلسلة المحرفية، الأول يستخدم الصيغة الجديدة، بينما التعبير الثاني والثالث يستخدمان الصيغ القديمة:

```javascript
const str = "hello this جملة is used for اختبار regex Unicode escapes.";

const regexArabic = /\p{Script=Arabic}+/gu;
console.log(str.match(regexArabic));
// expected output: Array ["جملة", "اختبار"]

const oldRegex = /[\u0621-\u064A]+/gu;
console.log(str.match(oldRegex));
// expected output: Array ["جملة", "اختبار"]

const oldRegex2 = /[ء-ي]+/gu;
console.log(str.match(oldRegex2));
// expected output: Array ["جملة", "اختبار"]
```

التعامل مع الصيغة الجديدة أسهل ولا حاجة لمعرفة مجالات الحروف المراد مطابقتها، كما أنها تحدث تلقائياً مع الزمن كلما طرأت تغييرات وتحديثات على معيار يونيكود.

تكتب هذه الصيغة بالشكل <code>&lrm;\p{ ... }&lrm;</code> (حرف `p` صغير) للمطابقة مع الخاصية التي تكتبها بين القوسين، أو بالشكل <code>&lrm;\P{ ... }&lrm;</code> (حرف `P` كبير) لنفي التطابق مع الخاصية المحددة بين القوسين.

يمكنك مطابقة المحارف حسب اللغة (Script)، أو حسب خاصية يونيكود (Unicode property). من الخصائص المدعومة: 

```javascript
const sentence = 'A ticket to 大阪 costs ¥2000 👌.';

const regexpEmojiPresentation = /\p{Emoji_Presentation}/gu;
console.log(sentence.match(regexpEmojiPresentation));
// expected output: Array ["👌"]

const regexpNonLatin = /\P{Script_Extensions=Latin}+/gu;
console.log(sentence.match(regexpNonLatin));
// expected output: Array [" ", " ", " 大阪 ", " ¥2000 👌."]

const regexpCurrencyOrPunctuation = /\p{Sc}|\p{P}/gu;
console.log(sentence.match(regexpCurrencyOrPunctuation));
// expected output: Array ["¥", "."]
```

### # [استخدام عامل التفكيك والنشر `...` مع الكائنات](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Destructuring_assignment)

عامل النقط الثلاث `...` يسمح بتفكيك المصفوفات والكائنات إلى مكونات فرعية أثناء عملية الإسناد أو تعريف متغير جديد. كما يسمح بنشر محتويات المصفوفة وتمريرها في الأماكن التي تتوقع استقبال صفر أو أكثر من العناصر (مثل استدعاء دالة أو تعريف مصفوفة جديدة)، أو نشر محتويات الكائن وتمريرها بشكل مجموعة من أزواج key-value إلى التعليمات التي تتوقع استقبالها.

الجديد في ES2018 هو دعم التعامل مع الكائنات (سابقاً كان يعمل مع المصفوفات وبارامترات الدوال).

```javascript
let obj1 = { foo: 'bar', x: 42 };
let obj2 = { foo: 'baz', y: 13 };

let clonedObj = { ...obj1 };
// Object { foo: "bar", x: 42 }

let mergedObj = { ...obj1, ...obj2 };
// Object { foo: "baz", x: 42, y: 13 }
```

عامل النشر يتجاهل القيم البوليانية الخاطئة (falsey values)، وهذا يسمح بدمج بعض الخصائص في كائن جديد شرطياً:

```javascript
const trueCondition = true;
const falseCondition = false;

const obj = {
  ...(trueCondition && { dogs: "woof" }),
  ...(falseCondition && { cats: "meow" }),
};
// obj = { dogs: 'woof' }
```

### # [Promise.prototype.finally](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise/finally)

هذا المُتناوِل handler يستقبل دالة تستدعى عند حسم نتيجة الوعد، رفضاً أو تحققاً. يفيد استخدامه لتجنب تكرار الكود الذي نريد تنفيذه في حالتي `then` و `catch`. 

```javascript
p.finally(function() {
   // settled (fulfilled or rejected)
});
```

### # [Asynchronous Iteration](https://2ality.com/2016/10/asynchronous-iteration.html)

إضافة المكررات غير المتزامنة. هناك [مقالة تشرحها على أكاديمية حسوب](https://academy.hsoub.com/programming/javascript/%D8%A7%D9%84%D9%85%D9%83%D8%B1%D8%B1%D8%A7%D8%AA-iterators-%D9%88%D8%A7%D9%84%D9%85%D9%88%D9%84%D8%AF%D8%A7%D8%AA-generators-%D8%BA%D9%8A%D8%B1-%D8%A7%D9%84%D9%85%D8%AA%D8%B2%D8%A7%D9%85%D9%86%D8%A9-%D9%81%D9%8A-%D8%AC%D8%A7%D9%81%D8%A7%D8%B3%D9%83%D8%B1%D8%A8%D8%AA-r923/).


<style>
html, body {
  direction: rtl;
}
pre, code {
  direction: ltr;
  text-align: left;
}
</style>
