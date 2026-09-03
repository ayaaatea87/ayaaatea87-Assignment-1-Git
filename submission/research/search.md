## git search 

1- squash

(
use commit , but meld into pervious commit
)

بمعنى ان ادمج
اكتر من commit مع بعض داخل commit واحدة فقط

مثال :

A→B→C→D→E↖Head

لو افترضنا ان
الخمسة commit
مرتبطين بنفس ال feature ,

ممكن ادمج اخر 4 commit مع بعض بحيث :

A→X

نقدر نعمل كده من خلال استخدام command التالى :

git rebase -i HEAD~n

( n : هي عدد ال commit )

هيفتح شاشة
فيها 4commit :

Pick B

Pick C

Pick D

Pick E

هنعدل اخر 3pick to squash

Pick B

Squash C

Squash D

Squash E

هيدمج C,D,E بداخل B فى النهاية :

(A→B ( ONLY 2 COMMIT

ال squash بيغيرال history وكمان بيغير ال ID الخاص بال commit , بيكريت ID جديد خاص بال commit الجديدة

النقطة دى ممكن تسبب مشاكل لو commit مرفوعة remote

الافضل استخدام امر ال squash قبل ال pull requset

2/ merge vs rebase
Merge

( main) A→B→C

↓D→E ( feature )

هنا عايزة اضيف الcommit
الخاصة بال feature branch على الmain branch هنعمل كده بطريقتين :

اول طريقة
وهى ال
mrege :

git switch main

git merge feature

( main) A→B→C→ M

↓D→E↗

عملنا commit جديدة والميرج تم بطريقة 3way merge

الميزة هنا ان history التفرع متغيرش على ال workflow فقط تم اضافة نقطة commit جديدة

rebase :

تانى طريقة هي استخدام :

git switch feature

git rebase main

A→B→C→D′ → E′

فى الحالة دى git بياخد الcommits الخاصة بالfeature branch ويضعها دايركت على الmain branch

لكن هنا يتم تغير الIDs الخاصة بالcommit وكذلك تغيير الhistory

الrebase مفيدة فى البروجكت الكبيرة حيث وجود history clean بدون تفرعات وبالتالى اسهل للقراءة

3/ git help

يستخدم فى حالة لو عايز اعرف تفاصيل اكتر عن command معين

مثال:

git merge – help // git help merge

بيفتح documentation فيها شرح merge command

4/ git clean

بيمسح ال untracked files

5/ git shortlog

بيعرض الcommits حسب الauthor وده مفيد في حالة التيم، لو عايز اعرف كل شخص عمل كام commits وايه هي الcommits دي

6/ git cherry-pick

معناها انى باخد التغييرات الموجود فى commit معين وبضيفها فى الbranch بتاعى

السيناريو هنا : لو شغال على feature معين على branch منفصل , ومحتاج تغييرات خاصة بحل bugs من commit موجود على branch تانى مختلف ، من غير ما اضطر اعمل merge

هنا هعمل apply or copy لل commit ده داخل الbranch بتاعى

مش بنقل الcommit ، فقط بعمل apply ل content داخل commit جديدة واضيفها على ال branch الخاص بيا

How to use this command :
1/

Move to the branch where you want to apply the copied changes :

git switch my-feature

2/

Apply the commit to your current branch using the copied hash

git cherry-pick a1b2c3d

7/ git bisect

تستخدم فى حالة لو ظهر bugs فى البروجكت وعايز اعرف انهى commit كانت السبب فى وجوده ، والجيت بيستخدم الجوريزم الbinary search علشان اوصل ل commit اللى سبب المشكلة

نستخدم الامر كالاتى :

1/

git bisect start

2/

git bisect bad

النسخة الحالية فيها مشكلة

3/

git bisect good A

بحدد commit متأكدة انها سليمة علشان يبدأ سيرش من عندها

4/

git bisect reset

8/ git prune

هى عبارة عن an internal housekeeping tool تستخدم لحذف الملفات او ال old commit او ال trees او branches الغير مستخدمة ( unreachable objects ) من الداتا بيز

سيناريو : فى حالة استخدام rebase command

A→B→C→D′ → E′

↓D→E

⇒ ال D,E هنا مافيش حاجة بتشاور عليهم وبالتالى من ضمن unreachable data

فى الحالة دى ممكن نمسحهم باستخدام git prune

الداتا اللى اتمسحت cannot be recovered

9/ git worktree

الفكرة هنا انى اتنقل بين برانشز مختلفة فى نفس ال repo فى نفس الوقت بدون استخدام stash

عن طريق عمل working directly منفصل لكل برانش داخل البروجكت

السيناريو هنا مثلا لو شغال على pay feature وحصل bug فى الmain بدل ما اعمل stash للشغل الموجود فى برانش payment بستخدم الامر :

git worktree add ../payment/pay-feature

10/ git verify-commit

التأكد من ال signature الموجودة على الcommit مفيدة فى حالة المشاريع الكبيرة لو طبقنا فكرة signed commit لكل يوزر في التيم بحيث اقدر اتأكد من صحة commit معينة قبل استخدامها خلال البروجكت

11/ git filter-repo

تستخدم لاعادة كتابة ال Git repository history وده مهم احيانا فى حالة لو عملت commit لداتا Sensitive مسح الملف نفسه مش كفاية لان ممكن حد يرجعه من commit قديمة

فى الحالة دى ضرورى امسحه من ال history

مسح الhistory بيغير الIDs الخاصة ب commits وبالتالى ده ممكن يأثر لو الشغل موجود remotly

12/ git grep

تستخدم فى البحث عن كلمة او نص معين داخل البروجكت ، مثلا عايز اعرف استخدمت jwt فين ؟

( git grep jwt )

13/ git blame

بتعرف من خلالها مين كتب السطر ده فى ملف ما

git blame author.js

بترجع كل سطر فى الكود مين اللى كتبه وايه الhash commit الخاصة بيه وتاريخ التعديل