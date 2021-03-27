---
title: "الجديد في إكماسكربت 2019"
date: 2021-03-27
---

نستعرض هنا ملخصاً للتعديلات الجديدة على توصيف إكماسكربت عام 2019، هذه المقالة جزء من مجموعة تغطي مجمل الإضافات والتعديلات الداخلة على المعيار القياسي للغة جافاسكربت في السنوات الماضية.

[تعديلات إكماسكربت عام 2020](https://muhammetsait.com/articles/new-in-es2020/)

[تعديلات إكماسكربت عام 2021](https://muhammetsait.com/articles/new-in-es2021/)


## إضافات ES2019

### # [Object.fromEntries](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/fromEntries)

تأخذ هذه الدالة مجموعة أزواج [مفتاح، قيمة] وتنشئ كائناً جديداً منها. مجموعة الدخل يمكن أن تكون مصفوفة أو [Map](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Map) أو أي كائن [Iterable](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Iteration_protocols#the_iterable_protocol).

```javascript
const obj = Object.fromEntries([
  ['foo', 'bar'],
  ['baz', 42]
]);

console.log(obj);
// expected output: Object { foo: "bar", baz: 42 }

```

### # [String.prototype.{trimStart,trimEnd}](https://github.com/tc39/proposal-string-left-right-trim)

تستخدم الدالة [`trimStart`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/trimStart) لإزالة المحارف الفارغة من بدايات السلاسل المحرفية ( مثل محرف المسافة أو المسطرة space، علامة الجدولة tab، المسافة غير المنقسمة non-breaking space، وغيرها بالإضافة لإزالة محارف السطر الجديد `CR` `LF` الخ)، أما [`trimEnd`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/trimEnd) فتزيلها من نهايات السلاسل المحرفية.

الأسماء القياسية هي `trimStart`/`trimEnd`، لكن  هناك اسمين مستعارين لهاتين الدالتين هما `trimLeft`/`trimRight` تم الحفاظ عليهما لأغراض التوافق مع بعض المتصفحات التي أضافت هذه الدوال قبل إدراجها في المعيار القياسي في 2019.

دالة [`trim`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/Trim) التي تحذف المحارف الفارغة من طرفي السلسة سبقت إضافتها في ES5 عام 2009.

### # [Array.prototype.{flat,flatMap}]()

إضافة دالتي [`flat`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/flat) و [`flatMap`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/flatMap) إلى كائن المصفوفة.

دالة `flat` تفرد عناصر المصفوفات الفرعية وتضمها إلى المصفوفة الأم التي تحويها (تعيد الناتج بشكل مصفوفة جديدة ولا تعدل على مصفوفة الدخل). تأخذ الدالة برامتراً يحدد عمق المصفوفات التي سيتم دمجها في مستوى واحد.

```javascript
const arr1 = [0, 1, 2, [3, 4]];

console.log(arr1.flat());
// expected output: [0, 1, 2, 3, 4]

const arr2 = [0, [1, 2], 5, [[11, [3, 4]], 12], 6];
 
console.log(arr2.flat(2));
// expected output: [0, 1, 2, 5, 11, [3, 4], 12, 6]
```

دالة `flatMap` تكافئ استدعاء [`map`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/map) يتبعها استدعاء `flat(1)` على المصفوفة الناتجة.

### # [Optional catch binding](https://github.com/tc39/proposal-optional-catch-binding)

هذا التعديل يسمح لك بكتابة الصيغة التالية:

```javascript
try {
...
} catch {
...
}
```

بدلاً من:

```javascript
try {
...
} catch (err) {
...
}
```

الصيغة الأولى الجديدة مفيدة في حال لم تكن تستعمل المتغير `err` في بلوك `catch` أو إذا كنت تريد تجاهل أي نوع من الأخطاء تماماً بإضافة بلوك `catch` فارغ، حيث لم تعد هناك ضرورة لتعريف المتغير إذا لم ترغب باستخدامه. 

### # [JSON superset](https://github.com/tc39/proposal-json-superset)

رغم أن إكماسكربت تسمح بكتابة JSON بشكل مباشر ضمن الكود المصدري، إلا أن هناك محرفين (هما U+2028 ـ LINE SEPARATOR و U+2029 ـ PARAGRAPH SEPARATOR) يسمح بكتابتهما في السلاسل النصية في JSON دون تهريب لكنهما ممنوعان في السلاسل النصية في إكماسكربت.

هذا التعديل جعل هذين المحرفين مسموحين، وبالتالي يمكن تضمين أي JSON سليم ضمن أكواد إكماسكربت دون مشاكل. بهذا يصبح JSON مجموعة جزئية محتواة في إكماسكربت (أي أن JSON ⊂ ECMAScript).

### # [Symbol.prototype.description](https://tc39.es/proposal-Symbol-description/)

إضافة خاصية `description` إلى كائنات الرموز [`Symbol`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Symbol)، هذه الخاصية تعيد وصف الرمز بشكل مباشر بدلاً من الاضطرار للوصول إليها بشكل غير مباشر باستخدام `toString`. 

```javascript
Symbol('desc').toString();   // "Symbol(desc)"
Symbol('desc').description;  // "desc"
Symbol('').description;      // ""
Symbol().description;        // undefined
```

### # [مراجعة Function.prototype.toString](https://2ality.com/2016/08/function-prototype-tostring.html)

تعديل على طريقة عمل دالة `toString` عند استدعائها على الدوال. الهدفان الأساسيان هما إعادة النص المصدري للدالة في حال كان ذلك ممكناً، بصيغة سليمة وقابلة للتنفيذ. الهدف الثاني، في حال تعذر إعادة شفرة برمجية سليمة، يجب إعادة سلسلة غير صالحة نحوياً، أي أنها ستسبب `SyntaxError` في حال محاولة تنفيذها باستخدام [`eval`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/eval).


### # [Well-formed JSON.stringify](https://github.com/tc39/proposal-well-formed-stringify)

تصحيح مشكلة في عمل دالة [`JSON.stringfy`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/JSON/stringify) حيث كانت تعيد سلاسل نصية غير متوافقة مع معيار UTF-8 ولا معيار UTF-16 إذا وجدت بعض المحارف الخاصة (ضمن النطاق U+D800 حتى U+DFFF) في بيانات الدخل.



<style>
html, body {
  direction: rtl;
}
pre, code {
  direction: ltr;
  text-align: left;
}
</style>
