[![Dependabot Status](https://api.dependabot.com/badges/status?host=github&repo=dankmitchell/spina-articles)](https://dependabot.com)

# Getting Started

This is a News/Blog plugin for Spina CMS based on articles.

```
gem 'spina-articles'
```

Run the migration installer to get started.

```
rails g spina_articles:install
```

This should copy the migration file required to create the Spina::Article model.

Restart your server and head over to '/admin', you should see your plugin located below the Media Library.

That's all it takes to get the plugin working :)

### Consumer views

##### Note: This plugin does not include the controller or view for the customer facing website, just the admin only.

To get you going you'll need to add the routes:

```ruby
Spina::Engine.routes.draw do
  resources :articles, only: [:show, :index], path: 'blog'
end
```

Then create the controller `controllers/spina/articles_controller.rb` and include your index and show actions:

```ruby
module Spina
  class ArticlesController < ApplicationController
    before_action :set_page
    layout 'layouts/default/application'

    def index
      @articles = Spina::Article.order(created_at: :desc).all
    end

    def show
      @article = Spina::Article.friendly.find(params[:id])
    end

    def set_page
      @page = Spina::Page.find_or_create_by(name: 'blog') do |page|
        page.title = 'Blog'
        page.link_url = '/blog'
        page.deletable = false
      end
    end
  end
end
```

Finally, create your index and show views in `views/spina/articles` folder

### Locales

You can override the locales file for further customization:

```ruby
en:
  spina:
    articles:
      scaffold_name: Career
      scaffold_name_plural: Careers
      title: ! '%{scaffold_name}'
      all: ! 'All %{scaffold_name}'
      new: ! 'New %{scaffold_name}'
      save: ! 'Save %{scaffold_name}'
      delete_confirmation: "Are you sure you want to delete <strong>%{subject}'s</strong> message?"
      empty: ! 'No %{plural} Yet'
```

## Page part

You can use `articles` on standard Spina pages by adding page part like the below example

```ruby
{ name: 'featured_article', title: 'Featured article', partable_type: 'Spina::ArticlePart' }
```

This project rocks and uses MIT-LICENSE.
