# Django is python web-framework 
-- In this we use MVT other than MVC like springboot in this we generally do one thing create veiw set and set serializers to do the validation stuff okay.

Initializing the framework using these commands:
    django-admin startproject myproject . 
    cd myproject
    django-admin startapp myapp .


Architecture for django and internal system working:

Model (The Kitchen/Pantry): Where the data (ingredients) lives. It defines the structure of your database.

View (The Waiter): The logic. It takes a request from the user, talks to the model to get data, and decides what to show.

Template (The Presentation): How the food is plated. It's the HTML/CSS that the user actually sees.


The request response cycle:
