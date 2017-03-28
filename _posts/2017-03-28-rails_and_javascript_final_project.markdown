---
layout: post
title:  "Rails & JavaScript Final Project"
date:   2017-03-28 18:48:15 +0000
---


Having finished my final JavaScript project, I definitely feel like I have come a long way. It is really cool seeing all of the stuff that I have learned come together. When I first started working on this project, I had a lot of trouble because I didn’t feel too confident with JavaScript. I felt like I had kind of breezed through the JavaScript lessons without really taking the time to understand what I was learning and how to actually apply it. Due to that, when I started this project, I had no idea where to begin. Finally, I decided that the best thing would be to redo the JavaScript lessons that I was shaky on and I am so glad that I did. After redoing some of the lessons, I feel so much more confident using JavaScript and I definitely have a better appreciation for it as a whole! So here is how I applied it to my fitness tracking application that I created for my final Rails project.

## The Show Resource

One of the requirements for this project was to render a show resource using jQuery and an Active Model Serialization JSON backend. I decided to make the workout show page have a ‘Next Workout’ button and a ‘Previous Workout’ button. Basically, when either of these buttons are clicked, the next or previous workout will be shown without reloading the page. Using these buttons, you can traverse through all of the workouts in either direction without refreshing.

So how does this work? There is a little more that goes in to rendering the show resource than what I am about to explain, but this will cover the main points of the JavaScript. I’ll use the Next button to explain. Basically, I tied an event listener onto the button so that when it is clicked, the Id of the next workout is saved and a request is made.

```$.get("/workouts/" + nextId + ".json", function(data) {
	// TO DO
}```

The data sent to the function contains the serialized workout from which we can get information about the workout. 

```
    $.get("/workouts/" + nextId + ".json", function(data) {
      $(".workoutDate").text(data["created_at"]);
      $(".workoutContent").text(data["content"]);
      $(".workoutCalories").text(data["calories_burned"]);
      // reset the id to the new current id
      $(".js-next").attr("data-id", data["id"]);
    });
```

Basically all that this block of code is doing is getting information about the workout from the json received on workouts/nextID.json and filling in divs on the page with the new information.

## The Index Resource

Another requirement is to render an index resource. Since there is so much nutrition information associated with a food item, I decided to use a ‘More Info’ button on the meals index page to allow more nutrition information to be displayed for each food item. This is pretty similar to what I did for the show resource.

Here is the main part of the JavaScript code that I used for this part. I will walk you through what’s happening.

```
<script type="text/javascript" charset="utf-8">
$(function () {
  $(".js-more").on('click', function() {
    var id = parseInt($(".js-more").attr("data-id"));
    $.get("/meals/" + id + ".json", function(data) {
      var foodItems = data["food_items"];
      var foodList = "";

      foodItems.forEach(function(food) {
        foodList += '<li>Name: ' + food["name"] + '</li><br><li>Serving Size: ' + food["serving_size"] + '</li><br><li>Number of Servings: ' + food["num_servings"] + '</li><br><li>Fat: ' + food["fat"] + '</li><br><li>Cholesterol: ' + food["cholesterol"] + '</li><br><li>Sodium: ' + food["sodium"] + '</li><br><li>Carbs: ' + food["carbs"] + '</li><br><li>Protein: ' + food["protein"] + '</li><br>';
      });
      $("#meal-" + id + "-food_items").html(foodList);
      $(".js-more").hide();
    });
    return false;
  });
});
</script>
```
First, we have a function tied to the button with the class js-more (AKA the ‘More Info’ button) being clicked. When this button is clicked, the id of the meal is saved and a request is made to /meals/id.json. This is exactly like what we did for the show resource so far, except with meals instead of workouts. Just like with the workouts, we are still going to get our information from data.

Each meal has many food items, so thanks to the Active Model Serializer, the food items can be found in `data[“food_items”]`
Next, we iterate through the food items and we create a list in HTML of all the nutrition facts within the food item and save that list as foodList. Lastly, we put the html of foodList in an unorganized list with the Id of meal-id-food_items and we hide the ‘More Info’ button since it has already been clicked.

## In Conclusion

I only went through two of the main requirements for the project in this blog post, but I think that they accurately represent the bulk of the project.  I thought that I would hate this project because at first I didn’t like JavaScript, but after redoing many of the lessons I actually really enjoyed this project. I feel like I learned so much and like I am much more comfortable with JavaScript now!


