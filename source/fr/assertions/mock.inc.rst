.. _mock-asserter:

mock
****

C'est l'assertion dédiée aux bouchons.

.. code-block:: php

   <?php
   $mock = new \mock\MyClass;

   $this
       ->mock($mock)
   ;

.. note::
   Reportez-vous à la documentation sur :ref:`les bouchons (mock) <les-bouchons-mock>` pour obtenir plus d'informations sur la façon de créer et gérer les bouchons.


.. _call-anchor:

call
====

``call`` permet de spécifier une méthode du mock à tester, son appel doit être suivi d'un appel à une méthode de vérification d'appel comme `atLeastOnce`_, `once/twice/thrice`_, `exactly`_, etc...

.. code-block:: php

   <?php

   $this
       ->given($mock = new \mock\MyFirstClass)
       ->and($object = new MySecondClass($mock))

       ->if($object->methodThatCallMyMethod())  // Cela va appeler la méthode myMethod de $mock
       ->then

       ->mock($mock)
           ->call('myMethod')
               ->once()
   ;

.. _at-least-once:

atLeastOnce
```````````

``atLeastOnce`` vérifie que la méthode testée (voir :ref:`call <call-anchor>`) du mock testé a été appelée au moins une fois.

.. code-block:: php

   <?php
   $mock = new \mock\MyFirstClass;

   $this
       ->object(new MySecondClass($mock))

       ->mock($mock)
           ->call('myMethod')
               ->atLeastOnce()
   ;

.. _exactly-anchor:

exactly
```````

``exactly`` vérifie que la méthode testée (voir :ref:`call <call-anchor>`) du mock testé exactement un certain nombre de fois.

.. code-block:: php

   <?php
   $mock = new \mock\MyFirstClass;

   $this
       ->object(new MySecondClass($mock))

       ->mock($mock)
           ->call('myMethod')
               ->exactly(2)
   ;

.. _never-anchor:

never
`````

``never`` vérifie que la méthode testée (voir :ref:`call <call-anchor>`) du mock testé n'a jamais été appelée.

.. code-block:: php

   <?php
   $mock = new \mock\MyFirstClass;

   $this
       ->object(new MySecondClass($mock))

       ->mock($mock)
           ->call('myMethod')
               ->never()
   ;

.. note::
   ``never`` est équivalent à ``:ref:`exactly <exactly-anchor>`(0)``.


.. _once-twice-thrice:

once/twice/thrice
`````````````````
Ces assertions vérifient que la méthode testée (voir :ref:`call <call-anchor>`) du mock testé a été appelée exactement :

* une fois (once)
* deux fois (twice)
* trois fois (thrice)

.. code-block:: php

   <?php
   $mock = new \mock\MyFirstClass;

   $this
       ->object(new MySecondClass($mock))

       ->mock($mock)
           ->call('myMethod')
               ->once()
           ->call('mySecondMethod')
               ->twice()
           ->call('myThirdMethod')
               ->thrice()
   ;

.. note::
   ``once``, ``twice`` et ``thrice`` sont respectivement équivalents à un appel à ``:ref:`exactly <exactly-anchor>`(1)``, ``:ref:`exactly <exactly-anchor>`(2)`` et ``:ref:`exactly <exactly-anchor>`(3)``.


.. _with-any-arguments:

withAnyArguments
````````````````

``withAnyArguments`` permet de ne pas spécifier les arguments attendus lors de l'appel à la méthode testée (voir :ref:`call <call-anchor>`) du mock testé.

Cette méthode est surtout utile pour remettre à zéro les arguments, comme dans l'exemple suivant :

.. code-block:: php

   <?php
   $mock = new \mock\MyFirstClass;

   $this
       ->object(new MySecondClass($mock))

       ->mock($mock)
           ->call('myMethod')
               ->withArguments('first')     ->once()
               ->withArguments('second')    ->once()
               ->withAnyArguments()->exactly(2)
   ;

.. _with-arguments:

withArguments
`````````````

``withArguments`` permet de spécifier les paramètres attendus lors de l'appel à la méthode testée (voir :ref:`call <call-anchor>`) du mock testé.

.. code-block:: php

   <?php
   $mock = new \mock\MyFirstClass;

   $this
       ->object(new MySecondClass($mock))

       ->mock($mock)
           ->call('myMethod')
               ->withArguments('first', 'second')->once()
   ;

.. warning::
   | ``withArguments`` ne teste pas le type des arguments.
   | Si vous souhaitez vérifier également leurs types, utilisez :ref:`withIdenticalArguments <with-identical-arguments>`.


.. _with-identical-arguments:

withIdenticalArguments
``````````````````````

``withIdenticalArguments`` permet de spécifier les paramètres attendus lors de l'appel à la méthode testée (voir :ref:`call <call-anchor>`) du mock testé.

.. code-block:: php

   <?php
   $mock = new \mock\MyFirstClass;

   $this
       ->object(new MySecondClass($mock))

       ->mock($mock)
           ->call('myMethod')
               ->withIdenticalArguments('first', 'second')->once()
   ;

.. warning::
   | ``withIdenticalArguments`` teste le type des arguments.
   | Si vous ne souhaitez pas vérifier leurs types, utilisez :ref:`withArguments <with-arguments>`.


.. _was-called:

wasCalled
=========

``wasCalled`` vérifie qu'au moins une méthode du mock a été appelée au moins une fois.

.. code-block:: php

   <?php
   $mock = new \mock\MyFirstClass;

   $this
       ->object(new MySecondClass($mock))

       ->mock($mock)
           ->wasCalled()
   ;

.. _was-not-called:

wasNotCalled
============

``wasNotCalled`` vérifie qu'aucune méthode du mock n'a été appelée.

.. code-block:: php

   <?php
   $mock = new \mock\MyFirstClass;

   $this
       ->object(new MySecondClass($mock))

       ->mock($mock)
           ->wasNotCalled()
   ;
