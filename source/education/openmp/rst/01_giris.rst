OpenMP’ye Giriş
===============

İş Parçacığı (Thread) Tabanlı Paralel Programlama
-------------------------------------------------

-  OpenMP’de paraleleşme tamamıyla iş parçacıkları (ing., thread)
   vasıtasıyla sağlanmaktadır.
-  İş parçacığı bir işletim sistemi tarafından çalıştırılabilen en küçük
   işlem parçasıdır.
-  Her iş parçacığı bir işlem’e (ing., process) aittir.
-  İş parçacığı sayısı yazılıma bağlıdır. Genelde paralel programlamada,
   işlemcinin çekirdek sayısı baz alınarak iş parçacığı sayısı
   ayarlanır.

Çatallanma (Fork-Join) Modeli
-----------------------------

-  OpenMP programları tek bir işlem altında tek bir iş parçacığı
   (ing., master thread) ile başlar.
-  Programcı tarafından belirlenmiş paralel alanlara gelindiğinde bu
   master thread çatallanarak (ing., fork) birden fazla iş parçağı
   oluşturur.
-  Paralel alan tamamlandıktan sonra bu iş parçacıkları birleşir ve
   program yine tek iş parçacığı ile devam eder.
-  Bir programda birden fazla paralel alan bulunabilir ve her alan
   farklı sayıda iş parçacığı kullanabilir.

.. figure:: /assets/openmp-education/images/fork_join.png
   :alt: Çatallanma modelini gösteren bir şekil. İlk paralel alan 3, ikinci paralel alan 2 iş parçacığı kullanacak şekilde ayarlanmış.

Çatallanma modelini gösteren bir şekil yukarıda verilmiştir. Şekilde ilk paralel alan 3, ikinci paralel alan 2 iş parçacığı kullanmaktadır. 