---
title: "الجديد في إكماسكربت 2020"
date: 2021-03-10
---

نستعرض هنا ملخصاً للتعديلات الجديدة على توصيف إكماسكربت عام 2020، هذه المقالة جزء من مجموعة تغطي مجمل الإضافات والتعديلات الداخلة على جافاسكربت حديثاً. يمكنك [مطالعة تعديلات ES2021 هنا](https://muhammetsait.com/articles/new-in-es2021/).

## إضافات ES2020

### # [String.prototype.matchAll](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/matchAll)

تستقبل هذه الدالة تعبيراً منتظماً [RegExp](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/RegExp) وتعيد مكرراً [Iterator](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Iterators_and_Generators) يمكننا من الوصول لكافة نتائج مطابقة التعبير المنتظم في السلسلة المحرفية. يجب أن يستعمل التعبير المنتظم المستخدم في البحث رمز `g`، وإذا تم تمرير كائن من آخر فسيتم تحويله إلى تعبير منتظم باستدعاء الباني <code>new RegExp()&lrm;</code>.

يمكن معالجة النتائج عبر حلقة [`for...of`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/for...of) على المكرر، أو بتحويله إلى مصفوفة عادية باستخدام عامل النشر [`...`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Spread_syntax) أو الدالة [`Array.from`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/from).

```javascript
const str = 'a complete sentence must express a complete thought.';

[...str.matchAll('complete')]
// expected result: [
//   ["complete", index: 2, input: "a complete sentence must express a complete thought.", groups: undefined],
//   ["complete", index: 35, input: "a complete sentence must express a complete thought.", groups: undefined]
// ]

Array.from(str.matchAll(/comp[a-z]*/g))
// expected result: same as above
```

### # [import()&lrm;](https://github.com/tc39/proposal-dynamic-import)

دالة الاستيراد الديناميكي، هي دالة جديدة تسمح باستيراد الملفات بنفس أسلوب تعليمة `import` ولكن مع بعض الاختلافات:

- السماح باستخدام القوالب الحرفية بالإضافة للسلاسل النصية العادية، هذا يسمح باستيراد أكواد مختلفة ديناميكياً اعتماداً على قيم بعض المتغيرات، كاختلاف لغة التطبيق على سبيل المثال:

  ```javascript
  import(`./language-packs/${langCode}.js`)
  ```

- يمكن استخدامها ضمن أي نوع من السكربتات في أي مكان وليس فقط ضمن الموديولات modules.

- تسمح باستيراد الأكواد شرطياً (ضمن تعليمة `if` مثلاً) أو استيراد وحدات ثانوية ضمن `try/catch` مع استمرار التطبيق بالعمل في حال عدم نجاح الاستيراد.

- استخدام دالة <code>import()&lrm;</code> لا يسبب اعتمادية على الوحدات التي يتم استيرادها. بالتالي لا يشترط تحميل الاعتمادية وتنفيذها قبل تنفيذ السكربت الذي يستوردها.

### # [BigInt](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/BigInt)

كائن يسمح بتمثيل الأعداد الصحيحة الأكبر من الحجم الأقصى الذي يمكن تخزينه في المتغيرات من نوع [`Number`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Number)، أي التي تزيد عن &lrm;2^53^-1&lrm;. يمكنك إنشاء كائن BigInt جديد بإضافة حرف `n` إلى نهاية الرقم، أو باستدعاء دالة البناء <code>BigInt()&lrm;</code>.

```javascript
const bigNum = 1000n;
const bigNum2 = BigInt("2000");
```

### # [Promise.allSettled](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise/allSettled)

تأخذ هذه الدالة مجموعة كائنات وعود ضمن كائن مكرر (مثل مصفوفة) وتعيد وعداً جديداً. تحسم نتيجة الوعد الجديد عندما تحسم (settle) نتائج كل الوعود المدخلة إما بالتحقق fulfill  أو الرفض reject.

عند معرفة كل النتائج، تحسم نتيجة الوعد الجديد الذي أعادته الدالة، ويعطي مصفوفة كائنات كل منها يمثل نتيجة من نتائج الوعود المدخلة.

```javascript
const promise1 = Promise.reject(3);
const promise2 = 42;
const promise3 = new Promise((resolve, reject) => {
  setTimeout(resolve, 2000, 'foo');
});

Promise.allSettled([promise1, promise2, promise3]).then((values) => {
  console.log(values);
});
// expected output: [
//   { status: "rejected", reason: 3 },
//   { status: "fulfilled", value: 42 },
//   { status: "fulfilled", value: "foo" },
// ]
```

إذا كانت الوعود المدخلة تعتمد على بعضها، فربما عليك استخدام `Promise.all` التي أضيفت في ES2015. الاختلاف هو أن تلك الدالة تعيد وعداً يرفض ما أن ترفض قيمة أحد الوعود المدخلة ولا تنتظر حسم بقية النتائج في تلك الحالة.

### # [globalThis](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/globalThis)

هذه القيمة تشير دوماً إلى الكائن العام `this`، أي أنها تمثل كائن `window` في المتصفحات، وكائن `self` في عاملات الويب [Web Workers](https://developer.mozilla.org/en-US/docs/Web/API/Worker)، وكائن `global` في Node.js.

### # [for-in mechanics](https://github.com/tc39/proposal-for-in-order)

تمت إعادة النظر في ترتيب التكرار على الكائنات عند استخدام حلقة [`for...in`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/for...in). في البيئات التنفيذية المتوافقة مع هذا التعديل، ستتوافق حلقة for-in مع الطرق الأخرى عند التكرار على الكائنات بدلاً من المعيار القديم حيث كان سلوك for-in مفتوحاً للتطبيق بصور مختلفة بين المتصفحات والبيئات المختلفة.

بشكل عام، حلقة `for...in` هي من التعليمات التي يفضل تجنبها بشكل كامل بسبب فائدتها المحدودة وتوفر تعمليات بديلة أفضل منها، مثل [`for...of`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/for...of) و [`forEach`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/forEach). 

### # [Optional Chaining](https://github.com/tc39/proposal-optional-chaining)

أضيف عامل جديد هو عامل الترابط الاختياري ( <code>?.&lrm;</code> )، الذي يدعى أيضاً عامل الوصول الآمن، أو عامل الوصول المشروط، أو عامل أمان القيم الفارغة في لغات البرمجة الأخرى. هذا العامل يسمح بمحاولة الوصول إلى فروع المتغيرات دون التحقق أولاً من أنها تحوي قيماً معرفة.

إذا كان المتغير الأصل معرفاً، ستعاد محتويات المتغير الفرعي، أما إذا كان الأصل غير معرف (قيمته null أو undefined)، فسوف تعاد القيمة undefined بدلاً من إطلاق خطأ.

يمكن استخدامه للوصول بشكل آمن إلى خاصية في كائن، أو قيمة في مصفوفة، أو استدعاء دالة على كائن، أو استدعاء دالة يحتمل ألا تكون معرفة.

```javascript
a?.b   // undefined if `a` is null/undefined, `a.b` otherwise.
a == null ? undefined : a.b

a?.[x]  // undefined if `a` is null/undefined, `a[x]` otherwise.
a == null ? undefined : a[x]

a?.b()  // undefined if `a` is null/undefined, calls `a.b()` otherwise.
a == null ? undefined : a.b()

a?.()  // undefined if `a` is null/undefined, invokes the function `a()` otherwise.
a == null ? undefined : a()
```

### # [Nullish coalescing Operator](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Nullish_coalescing_operator)

عامل استبدال المتغيرات غير المعرفة ( `??` )، يمكنك من استبدال قيمة المتغير بقيمة أخرى إذا وفقط إذا كانت قيمة المتغير هي null أو undefined.

يعمل هذا العامل بشكل مشابه لعامل (أو) المنطقية `||`، لكنه لا يستبدل القيم المعرفة التي تحول إلى قيمة false منطقية. يعني، قيم 0 أو السلسلة النصية الفارغة وغيرها من القيم لا تستبدل مع عامل `??`.

```javascript
const a = null ?? 'default string';
console.log(a);
// expected output: "default string"

const b = 0 ?? 42;
console.log(b);
// expected output: 0

const c = 0 || 42;
console.log(c);
// expected output: 42
```

### # [import.meta](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/import.meta)

تعليمة `import.meta` تسمح بالوصول إلى معلومات متعلقة بالسياق أو البيئة التي تم استدعاء وحدة جافاسكريبت وتنفيذها فيها.

يمكن على سبيل المثال استخدامها للوصول إلى رابط الاستدعاء url وأي بارمترات معرفة في ذلك الرابط، أو الوصول إلى خصائص على وسم `<script>` الذي استدعى الملف، أو معرفة مسار الملف المستدعى في نظام الملفات الجهاز المستضيف، وغير ذلك من معلومات بيئة تنفيذ الموديول.

```javascript
<script type="module">
import './index.mjs?someURLInfo=5';
</script>


// in the file: index.mjs
new URL(import.meta.url).searchParams.get('someURLInfo');
// expected result: 5
```

مثال آخر:

```javascript
<script type="module" src="mymodule.mjs" data-size="500"></script>

// inside mymodule.mjs
const size = import.meta.scriptElement.dataset.size;
// expected result: size = 500
```


<style>
html, body {
  direction: rtl;
}
pre, code {
  direction: ltr;
}
</style>

