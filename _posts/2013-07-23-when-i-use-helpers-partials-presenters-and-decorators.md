---
layout: post
title: When I use Helpers, Partials, Presenters and Decorators
category: english
comments: true
---

There are some differents ways to organize your views' code in a Ruby On Rails application.
Partials and helpers are the standard methods, but there also are Presenters and Decorators.
It can be a little bit confusing to know how and when to use it.

My organization
===========

Every method have his own utility, and your mileage or taste may vary, but here's how I do mine.

Helpers
-------

Helpers are bits of Ruby code returning HTML, so that's returning strings with DOM, standard text or CSS style.

I use them as generic methods for all kind of objects.
Mine are like `link_to_update`, `big_image`, `styled_form`, etc.


Partials
-------

Partials are just like Ruby views, but you can call and render them in other views.
They're used to split a big view into smaller logic parts.

I can have a partial `side_menu`, `comment_list`, `header`, etc.

Presenters
----------
Presenters are for more complicated queries with two or more models.

I have some presenters like `@page_presenter.page_in_category(ruby_category)` or `@user_presenter.user_following(an_article)`.

I have only one thing which I don't like for presenters: I don't like to instanciate an object in the controller and to pass it into the view.
It does not respect the [Sandi Metz's rules](http://robots.thoughtbot.com/post/50655960596/sandi-metz-rules-for-developers).
But then, any of these rules can be broken with a good reason...


Decorators
----------
Decorators should act with only one model and shouldn't take parameters (if it's possible).
I can do something like this `user.full_name`, `page.big_title` or `category.permalink`.
I use the gem [Draper](https://github.com/drapergem/draper).

If I search many models, I do not access to the model class in the view.
I use draper's function [decorates_finders](https://github.com/drapergem/draper#decorated-finders).


Conclusion
----------
There may be a better set of solutions but these work for me.
If you have a better solution, please, let me know.

