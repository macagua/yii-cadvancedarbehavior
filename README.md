# yii-cadvancedarbehavior
Fork of Yii 1.1 CAdvancedArBehavior (Advanced functions for Active Record implementation) by thyseus.

## Yii 1.1: cadvancedarbehavior

The CAdvancedArBehavior extension adds up some functionality to the
default possibilites of yii´s ActiveRecord implementation. At the moment
it is able to automatically save MANY\_MANY relation objects when
save()-ing an Object.

### Changelog [¶](#hh0)

Version 0.3 added 25. 05. 2011 by thyseus

-   added \$ignoreRelations to ignore specified relations. The Behavior
    will take all found many2many relations by default. Specify
    exceptions in this array.
-   fixes all the bugs and glitches found in the discussion

### Resources [¶](#hh1)

-   [Join
    discussion](http://www.yiiframework.com/forum/index.php?/topic/6905-please-test-my-ar-enhancement-automatically-sync-many-many-table-when-calling-save/)

Documentation [¶](#hh2)
-----------------------

### Requirements [¶](#hh3)

-   Yii 1.1 or above

### Installation [¶](#hh4)

To use this extension, just copy this file to your extensions/
directory, add 'import' =\>
'application.extensions.CAdvancedArBehavior', [...] to your
config/main.php and add this behavior to each model you would like to
inherit the new possibilities.

### Usage [¶](#hh5)

    public function behaviors(){
              return array( 'CAdvancedArBehavior' => array(
                'class' => 'application.extensions.CAdvancedArBehavior'));
              }

### Possibilities so far: [¶](#hh6)

### Better support of MANY\_TO\_MANY relations: [¶](#hh7)

When we have defined a MANY\_MANY relation in our relations() function,
we are now able to add up instances of the foreign Model on the fly
while saving our Model to the Database. Let´s assume the following
Relation:

Post has: 'categories'=\>array(self::MANY\_MANY, 'Category',
'tbl\_post\_category(post\_id, category\_id)')

Category has: 'posts'=\>array(self::MANY\_MANY, 'Post',
'tbl\_post\_category(category\_id, post\_id)')

Now we can use the attribute 'categories' of our Post model to add up
new rows to our MANY\_MANY connection Table:

    $post = new Post();
    $post->categories = Category::model()->findAll();
    $post->save();

This will save our new Post in the table Post, and in addition to this
it updates our N:M-Table with every Category available in the Database.

We can further limit the Objects given to the attribute, and can also go
the other Way around:

     $category = new Category();
     $category->posts = array(5, 6, 7, 10);
     $category->save();

We can pass Object instances like in the first example, or a list of
integers that representates the Primary key of the Foreign Table, so
that the Posts with the id 5, 6, 7 and 10 get´s added up to our new
Category.

5 Queries will be performed here, one for the Category-Model and four
for the N:M-Table tbl\_post\_category. Note that this behavior could be
tuned further in the future, so only one query get´s executed for the
MANY\_MANY Table.

We can also pass a *single* object or an single integer:

     $category = new Category();
     $category->posts = Post::model()->findByPk(12);
     $category->posts = 12;
     $category->save();

Change Log [¶](#hh8)
--------------------

### January 30, 2010 [¶](#hh9)

Version 0.2 Code Cleanup, Bugfixes and added save() support

### January 28, 2010 [¶](#hh10)

-   Initial release.

