---
layout: post
title: Quand j'utilise des Helpers, des Partials, des Presenters et des Decorators
category: french
comments: true
---

Il existe plusieurs différentes façon d'avoir une meilleur organisation des vuews dans une application Ruby on Rails. Les Partials et les Heplers sont des méthodes standards. Il y a également les Presenters et les Decorators. Cela peut-être quelque peu difficile de savoir comment et quand les utiliser.

Mon oganisation
===========

Toutes les techniques ont leurs propre utilitée.

Les Helpers
-------

Les Heplers sont des méthodes génériques qui peuvent être utilisées pour différents types d'objet. Je créer cette sorte d'helper  `link_to_update`, `big_image`, `styled_form`, etc. Ces méthodes créent du code html avec, par exemple, un style css ou un texte standard.

Les Partials
-------

Les Partials sont utilisés pour diviser une grosse vue dans de plus petites parties logiques. Je peux avoir un partial `side_menu`, `comment_list`, `header`, etc.

Les Presenters
----------

Les Presenters sont créés pour des requêtes plus compliquées avec un modèle ou plus. J'ai des partials comme `@page_presenter.page_in_category(ruby_category)` ou `@user_presenter.user_following(an_article)`.

Les Decorators
----------

Les Decorators doivent intéragir avec un modèle seulement et ne doivent pas avoir de paramêtre (si possible). Je peux faire cette sorte de code : `user.full_name`, `page.big_title` ou `category.permalink`. J'utilise la Gem [Draper](https://github.com/drapergem/draper).

Si je cherche plusieurs modèles, Je n'accède pas à la classe du modèle directement dans la vue. J'utilise la fonction de Draper [decorates_finders](https://github.com/drapergem/draper#decorated-finders).

Conclusion
----------

Il peu y avoir de meilleurs solution mais ça marche pour moi. Si vous pensez avoir de meilleures solution, dites le moi!

Il y a juste une chose que je n'aime pas avec les presenters. Je n'aime pas instancier un objet dans le controlleur et le passer à la vue. Cela ne respecte pas la [règle de Sandi Metz](http://robots.thoughtbot.com/post/50655960596/sandi-metz-rules-for-developers). Toutes les règles peuvent être brisées avec des bonnes raisons...
