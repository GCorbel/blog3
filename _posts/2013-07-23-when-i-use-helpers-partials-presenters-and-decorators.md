---
layout: post
title: When I use Helpers, Partials, Presenters and Decorators
category: english
comments: true
---

There is some differents ways to have a better organization of your views in a Ruby On Rails application. Partials and helpers are the standard methods. There is also Presenters and Decorators. It can be a little bit confusing to know how and when to use it.

My organization
===========

Every technics can have his own utility.

Helpers
-------

Helpers are generic methods which can be use for different kind of objects. I create this kind of helpers `link_to_update`, `big_image`, `styled_form`, etc. Those methods create an html code with a css style or a standard text for example.

Partials
-------

Partials are used to split a big view into smaller logic parts. I can have a partial `side_menu`, `comment_list`, `header`, etc.

Presenters
----------

Presenters is for more complicated queries with two or more models. I have some partials like `@page_presenter.page_in_category(ruby_category)` or `@user_presenter.user_following(an_article)`.

Decorators
----------
Decorators should act with only one model and shouldn't take parameters (if it's possible). I can do something like this `user.full_name`, `page.big_title` or `category.permalink`. I use the gem [Draper](https://github.com/drapergem/draper).

If I search many models, I do not access to the model class in the view. I use the draper's function [decorates_finders](https://github.com/drapergem/draper#decorated-finders).

Conclusion
----------
There may be a better solution but it works for me. If you have a better solution, please, let me know.

I have only one thing which I don't like for presenters. I don't like to instanciate an object in the controller and to pass it into the view. It does not respect the [Sandi Metz's rules](http://robots.thoughtbot.com/post/50655960596/sandi-metz-rules-for-developers). Every rules can be broken with a good reason...
