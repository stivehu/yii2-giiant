yii2-giiant
===========

Extended models and CRUDs for Gii, the code generator of Yii2 Framework

**PROJECT IS IN DEVELOPMENT STAGE!**


What is it?
-----------

Giiant provides templates for model and CRUD generation with relation support and a sophisticated UI.
A main project goal is porting many features and learnings from gtc, giix, awecrud and others into one solution.


Installation
------------

The preferred way to install this extension is through [composer](http://getcomposer.org/download/).

    composer.phar require schmunk42/yii2-giiant:"*"

The generators are registered automatically in the application bootstrap process, if the Gii module is enabled

Usage
-----

Visit your application's Gii (eg. `index.php?r=gii` and choose one of the generators from the main menu screen.

For basic usage instructions see the [Yii2 Guide section for Gii](http://www.yiiframework.com/doc-2.0/guide-gii.html#how-to-use-it).


Features
--------

### Model generator

- generates separate model classes to customize and base models classes to regenerate
- table prefixes can be stipped off model class names (not bound to db connection setting)

### CRUD generator

- model, view and controller locations can be customized to use subfolders
- horizontal and vertical form layout
- action button class customization
- input, attribute, column and relation customization with provider queue
- callback provider to inject any kind of code for inputs, attributes and columns via dependency injection

#### Providers

- *CallbackProvider* universal provider to modify any input, attribute or column with highly flexible callback functions
- *RelationProvider* renders code for relations (eg. links, dropdowns)
- *EditorProvider* renders RTE, like `Ckeditor` as input widget
- *DateTimeProvider* renders date inputs

Customization with providers
----------------------------

In many cases you want to exchange i.e. some inputs with a customized version for your project.
Examples for this use-case are editors, file-uploads or choosers, complex input widget with a modal screen, getting
data via AJAX and so on.

With Giiant Providers you can create a queue of instances which may provide custom code depending on more complex
rules. Take a look at some existing [giiant providers](https://github.com/schmunk42/yii2-giiant/tree/develop/crud/providers).

Configure providers, add this to your provider list in the form:

    \schmunk42\giiant\crud\providers\EditorProvider,
    \schmunk42\giiant\crud\providers\SelectProvider,

And configure the settings of the provider, eg. add this to your config file:

    \Yii::$container->set(
        'schmunk42\giiant\crud\providers\EditorProvider',
        [
            'columnNames' => ['description']
        ]
    );

This will render a Ckeditor widget for every column named `description`.

    <?= $form->field($model, 'description')->widget(
    \dosamigos\ckeditor\CKEditor::className(),
    [
        'options' => ['rows' => 6],
        'preset' => 'basic'
    ]) ?>


### Universal `CallbackProvider`

Configuration via DI container:

```
\Yii::$container->set(
    'schmunk42\giiant\crud\providers\CallbackProvider',
    [

        'activeFields'  => [

           'common\models\Foo.isAvailable' => function ($attribute, $generator) {
               $data = \yii\helpers\VarDumper::export([0 => 'Nein', 1 => 'Ja']);
               return <<<INPUT

// Generate the code for a checkbox
\$form->field(\$model, '{$attribute}')->checkbox({$data});

INPUT;
           },
        ],


        'columnFormats' => [

           'common\models\Foo.html' => function ($attribute, $generator) {

               return <<<FORMAT

// generate custom HTML in column
[
    'format' => 'html',
    'label'=>'FOOFOO',
    'attribute' => 'item_id',
    'value'=> function(\$model){
        return \yii\helpers\Html::a(\$model->bar,['/crud/item/view', 'id' => \$model->link_id]);
    }
]

FORMAT;
           }

        ]
    ]
);
```

Generate the code for a checkbox:

```

```

Generate custom HTML in column:

```
```



Screenshots
-----------

TODO: update


Links
-----

