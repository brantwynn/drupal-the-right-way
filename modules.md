# Modules

Best practices for creating Drupal modules.

## Choosing a human-readable name

> There are only two hard things in Computer Science: cache invalidation and naming things.
>
> -- [Phil Karlton](http://martinfowler.com/bliki/TwoHardThings.html)

## Choosing a machine name

1. **Choose a machine name you can live with.** Changing a module's human-readable name is trivial. Changing it's machine name once you've installed it is hard. [Changing it once it's on Drupal.org](https://drupal.org/node/467944) and *thousands of people* have installed it is very hard. So choose as if the decision were final.

1. **Choose a machine name that corresponds to your module's human-readable name.** The relationship between your module's human-readable and machine names should be obvious. It's best if one can be inferred from the other. If your human-readable name is long, you can abbreviate, but don't go overboard, and make it predictable.

1. **Choose a machine name that's easy to remember, easy to type, and easy to say.** Unless you hate your users, don't make them check the filesystem or watch their fingers every time they type your module name; and don't make it painful to talk about your module with others or speak its function names. Don't use [leetspeak](http://en.wikipedia.org/wiki/Leet). Use whole words or well-accepted abbreviations, and be consistent about it. Think twice before being "clever".

1. **Make your machine name a valid file/directory name and a valid PHP label.** Your module machine name will be used in the module's directory-and-file names, so it must be [a valid Unix file name](http://www.december.com/unix/tutor/filenames.html). It will also be used to namespace all of your PHP functions, so it must be [a valid PHP label](http://us3.php.net/manual/en/functions.user-defined.php). In short, that means it must begin with a letter or an underscore, followed by any number of letters, numbers, or underscores. It shouldn't start with two underscores (__), because that signifies a magic function in PHP, and for greatest portability between operating systems you should avoid uppercase letters and funny Unicode characters.

1. **Make sure your machine name isn't already taken in your local codebase.** Drupal requires machine names to be unique across all project types--modules, themes, and installation profiles. Only one project will be recognized if there's duplication, so make sure there isn't. Don't forget about core projects and [Features](https://drupal.org/project/features) modules. The following Unix command run from your document root will identify probable duplication for a machine name of "example": `find -name example.info\*`.

1. **Make sure your machine name isn't already taken on Drupal.org.** Don't use a machine name that's already in use in contrib, even if you never intend to share your module with the public. It won't prevent other people from using your module, but it *will* prevent *you* from using the other project. The following Drush command will tell you whether there's a contrib project with the machine name "example": `drush pm-info example`. Absent Drush, you could browse to https://drupal.org/project/example.

1. **Make sure your machine name isn't prone to other collisions.** Drupal projects aren't the only source of namespaces. Subsystems such as the variable system and the cache system all prefix their functions (before D8) in such a way that you'll be competing with them if you choose a machine name of "variable" or "cache". If you choose "array", you'll be competing with [PHP's native array functions](http://www.php.net/manual/en/ref.array.php). In general, anything that defines PHP functions in the global namespace is a potential source of colissions.

1. **Don't use underscores.** Though many reputable modules have chosen to include underscores in their machine names, it seems best not to do so for the simple reason that underscores confuse your namespace. Grepping for function and variable names is less error-prone if namespaces are underscore-free. It's a slightly contrived example, but imagine trying to find all the functions belonging to views.module in a codebase that also contained views\_slideshow.module. `grep views_` would return them both. It's also easier to glance at a function name and filter out the namespace if there are no underscores in it. But most importantly, allowing underscores in your machine name results in essentially claiming multiple namespaces. For example, name your module "node\_file", and you've subtly polluted the "node" namespace. A function called `node_file_load()` *seems* like it's in your namespace, but declare it and you've accidentally implemented [`hook_file_load()`](https://api.drupal.org/api/drupal/modules!system!system.api.php/function/hook_file_load/7) on behalf of node module. The result will probably be a subtle bug that manifests in a part of the system that seems to have no relationship whatsoever to the code causing it. So you may like the looks of "my_awesome_module", but the argument for "myawesomemodule" is probably more compelling.
