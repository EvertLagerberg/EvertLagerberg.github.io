---
layout: post
title: Ruby on Rails:has_many-relations and nested models
---

Ruby on rails has a very easy convienent structure for building relations between objects of different classes. For example if you want to make it possible to relate many comments to many blogposts you can just:
Add a has_many statement to the Post model
Add a belongs_to statement to your Comment model

But for my Cookbook-app I wanted the possibility of adding a recipe-object and all the ingredient-objects in that recipe within the same form:

After some reading on this problem i found out that you can achieve this by using "accepts_nested_attributes_for. By putting "accepts_nested_attributes_for :ingredients" in to the recipe model I make this nestled form possible. Then you can use the field_for method in my View to add new recipes to be able to add atrributes to the associated ingredients for that recipe in that same form.

 Then i got stuck for a little bit, thinking too much about creating a controller for ingredients. In the end it turns out that i can do all I need from within the recipe controller. First I needed to update the recipe method new. Since new is passing POST information to the recipes/new-view:
  
def new
    @recipe = Recipe.new
    3.times { @recipe.ingredients.build }
  end

This way everytime I create a instancevariable containing a new recipe-object i also associate three ingredient-object with it. Then in the view, the field_for method creates input rows for each of these ingredients.

Next I needed to add the ingredients attributes to the recipe controllers create-method:

  def create
    @recipe = Recipe.new(params[:recipe].permit(:title, :desc, ingredients_attributes: [:name]))

    if @recipe.save
      redirect_to @recipe
    else
      render "new"
    end
  end

I got stuck on this for a while cause the way of doing this is different from rails 3 and rails  4 which I use, so most of the info i find online was missleading. This is something to consider for future problems!

This is a super useful railscast on the subject:
[http://railscasts.com/episodes/196-nested-model-form-revised](http://railscasts.com/episodes/196-nested-model-form-revised)
 


