---
title: "الجديد في إكماسكربت 2021"
date: 2021-03-10
---

نستعرض هنا ملخصاً شاملاً للمقترحات الجديدة التي دخلت على توصيف لغة إكماسكربت، الذي يعتبر المعيار القياسي الذي تتوافق معه جافاسكربت، عام 2021. هذه المقالة ستكون جزءاً من مجموعة تغطي مجمل الإضافات والتعديلات الداخلة على جافاسكربت حديثاً.

## إضافات ES2021

### # [String.prototype.replaceAll](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/replaceAll)

تأخذ هذه الدالة بارامترين، وتبحث ضمن السلسلة المحرفية عن كافة الأماكن التي ورد فيها البارامتر الأول وتستبدله بقيمة البارامتر الثاني. البارامتر الأول عبارة عن نمط بحث، قد يكون سلسلة محرفية أو تعبير منتظم [RegExp](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/RegExp) يستخدم للبحث. البارامتر الثاني قد يكون سلسلة نصية أو دالة يتم تنفيذها عند كل حالة مطابقة يعثر عليها.

تعيد دالة `replaceAll` ناتج الاستبدال في سلسلة تصية جديدة، أما السلسلة الأصلية فلا تجري عليها أي تغييرات.

إذا كان البارامتر الثاني دالة، فيمكنها استلام 3 بارامترات عند استدعائها عند كل حالة مطابقة: السلسلة النصية التي حققت التطابق، ورقم المحرف الذي وردت عنده هذه السلسلة (index)، والسلسلة المحرفية اﻷصلية كاملة.


```javascript
const p = 'Almost before we knew it, we had left the ground.';

console.log(p.replaceAll('we', 'they'));
// expected output: "Almost before they knew it, they had left the ground."

// global flag (g) id required when calling replaceAll with regex
// ignore case flag (i) is also used in this example
const regex = /a/ig;
p.replaceAll(regex, (a, b, c) => {console.log(a, b);})
// expected output:
// "A" 0
// "a" 30
```

### # [Promise.any](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise/any)

تأخذ هذه الدالة مجموعة كائنات وعود ([`Pormise`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise) objects) وترجع وعداً جديداً. يتحقق الوعد الجديد عندما يتحقق أول وعد من المجموعة المدخلة ويعيد قيمته نفسها.

إذا لم يتحقق أي وعد من المجموعة، ترجع الدالة وعداً مرفوضاً يعطي خطأ من نوع `AggregatorError`. أما في حال ظهور استثناء أثناء تنفيذ الوعود المدخلة، ترجع وعداً مرفوضاً مع إعادة الاستثناء الذي حصل.

```javascript
const promise1 = Promise.reject(0);
const promise2 = new Promise((resolve) => setTimeout(resolve, 100, 'quick'));
const promise3 = new Promise((resolve) => setTimeout(resolve, 500, 'slow'));

const promises = [promise1, promise2, promise3];

Promise.any(promises).then((value) => console.log(value));
// expected output: "quick"
```

وبذلك يصبح لدينا أربع دوال تجميع للوعود في إكماسكربت. كما في الجدول التالي:

| الدالة | وصف       | إصدار |
| -------------------- | ----------------------------------------------- | ------ |
| `Promise.allSettled` | تنتظر انتهاء جميع الوعود بالرفض أو التحقق       | ES2020 |
| `Promise.all`        | تنتهي عند رفض أول وعد                           | ES2015 |
| `Promise.race`       | تنتهي عند تحقق أو رفض أول وعد                   | ES2015 |
| `Promise.any`        | تنتهي عند تحقق أول وعد                          | ES2021 |

### # [WeakRefs](https://github.com/tc39/proposal-weakrefs)

هذا التعديل أضاف كائنين جديدين هما [`WeakRef`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/WeakRef) و [`FinalizationRegistry`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/FinalizationRegistry). يمكن استخدام هذين الكائنين للتحكم بعملية إدارة الذاكرة والتعامل مع جامع القمامة إلى حد ما. بشكل عام لا ينصح باستخدامهما إلا في حالات خاصة جداً، وهما موجهان للمكتبات وليس للاستخدام في أكواد التطبيقات.

### # [Logical Assignment Operators](https://github.com/tc39/proposal-logical-assignment/)

عوامل الإسناد المنطقية هي مجموعة عوامل تسمح باختصار بعض تعليمات الإسناد إلى شكل أقصر مع تحسين الأداء بشكل طفيف (لكن لا يغرنك الmicro-optimization). في المثال التالي، وضعنا كل تعليمة إسناد جديدة ويليها تعليمتين قديمتين: الأولى مكافئة تماماً، والثانية مكافئة في الناتج ولكنها تسبب عملية إسناد زائدة أحياناً.

```javascript
a ||= b;      // Setter is triggered only when `a` is falsey.
a || (a = b); // Ok
a = a || b;   // Ok, but setter is always triggered.

// "And And Equals"
a &&= b;
a && (a = b);
a = a && b;

// "QQ Equals"
a ??= b;
a ?? (a = b);
a = a ?? b;
```

### # [Numeric separators](https://github.com/tc39/proposal-numeric-separator)

أضيفت إمكانية استخدام الشرطة السفلية ( `_` ) للفصل بين الخانات عند تعريف الأعداد والثوابت، بهدف تسهيل قرائتها على المبرمج.

```javascript
let amount = 101_475_938.38; // a hundred million

let x = 0.000_001; // 1 millionth

let y = 1e10_000;  // 10^10000

let nibbles = 0b1010_0001_1000_0101; // with Binary numbers

let message = 0xA0_B0_C0; // with Hex values

let budget = 1_000_000_000_000n; // with BigInt number
```

<style>
html, body {
  direction: rtl;
}
pre, code {
  direction: ltr;
}
</style>
