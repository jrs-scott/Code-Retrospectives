# Python Live Project - Code Retrospective

## Table of Contents
* Introduction
* The Habit Tracker: starting an app from scratch
* Craigslist App: Adding improvements to an existing app
* Styling Changes: Front end updates to the main app
* Overall Summary

### Introduction
During my Python Live Project at the Tech Academy, I worked for two weeks with a development team of peers on a web application that facilitates a variety of microservices. The primary languages I used during this project were Python, HTML, CSS and the Django framework. Along with those languages I also utilized Git, Azure DevOps and Slack to coordinate with my team. I had the opportunity to create a new feature and improve existing apps. The user stories I completed involved both front and back end work, which provided a rounded development experience. 

### The Creation of a Habit Tracker App
The most substantial code I implemented was building an app that presented users the ability to make daily entries about their habits, and view the results of those entries displayed in a legible manner. This involved the following:
* Create a model which assigns the fields and data types for the database table
* Build a form to use the table structure from the database, and associate the data accordingly
* Style a UI that presents the data requested from the user in a tidy manner

Below is a code snip from the Django model I created. It associates the user to their entry form and provides various types of input for common habits they may want to track. 

```python
class Habit(models.Model):
   MOOD_CHOICES = ((1, '1'), (2, '2'), (3, '3'), (4, '4'), (5, '5'), (6, '6'), (7, '7'), (8, '8'), (9, '9'), (10, '10')) 
   ENERGY_CHOICES = ((1, '1'), (2, '2'), (3, '3'), (4, '4'), (5, '5'))

   user = models.ForeignKey(UserProfile, models.CASCADE)
   date = models.DateField(default=datetime.date.today)
   exercise = models.IntegerField(default=0, null=True)
   sleep = models.IntegerField(default=0, null=True)
   mood = models.IntegerField(null=True, choices=MOOD_CHOICES) # Presents a scale of 1-10
   energy = models.IntegerField(null=True, choices=ENERGY_CHOICES) # Presents a scale of 1-5
   meditate = models.BooleanField(default=False)
```

After the model and form were created, I implemented the input form as a home view for the app, and the user's stored data as a history output view:
```python
class HomeView(TemplateView):
    template_name = 'HabitTrackerApp/habit-home.html'

    def get(self, request):
        form = HabitEntryForm()
        return render(request, self.template_name, {'form' : form})

    def post(self, request):
        form = HabitEntryForm(request.POST)                
        if form.is_valid():
            form.save(request.POST)
            return redirect('history_view/')
        return render(request, self.template_name, {'form' : form})

def history_view(request):
    userEntries = Habit.objects.filter(user=request.user)
    return render(request, 'HabitTrackerApp/habit-history.html/', {'userEntries': userEntries})
```

Below is the initial styling for the habit history data requested from the user. This output page verified the user who made the request, addressed them directly in the header, and displayed relevant results. 
![History table results](https://github.com/jrs-scott/Code-Retrospectives/blob/master/results-page.JPG)

Creating the foundation of this application inspired me. If I had more time, I would have loved to add graphs for the data results instead of a table, as well as allowing the user to create custom habits to track. 

### Extend Functionality of the Craigslist Scraper App
One of the established apps took a request from the user and returned relevant sales listings from Craigslist. It started out by returning the title, price, and link to view it on the Craigslist site. I added the ability to also return the primary image from the listing using BeautifulSoup. Here is a code snip from that project:

```python
  # Loop over the gallery images from results and pull out first image sources
  for image in soup.find_all('a', class_='result-image gallery'):
      galleryString = (image['data-ids'])
      startID = galleryString.find('1:') + 2 
      endID = galleryString.find(',', startID)
      imageCode = galleryString[startID:endID] 
      src = "https://images.craigslist.org/%s_300x300.jpg" % imageCode # Extract the main image ID and create the image source
      images.append(src) # Populates the image sources to their corresponding list
```

### Front End Style Changes
Multiple developers had been adjusting the front end themes across the app without communicating the overall direction. This lead to inconsistent colors and layout across the web application. I completed multiple user stories that consolidated the differences and made the landing, login, and signup pages uniform in color scheme and layout. The goal was a very minimalistic, dark theme. I also added sonar animation effects to the buttons using CSS.
![Login styling](https://github.com/jrs-scott/Code-Retrospectives/blob/master/login-page.JPG)

## Summary
Working on this project gave me a lot of practical experience including:
* Completing user stories on deadlines
* Communicating with team members and the project manager to stay on the same page
* Further exposure to the Django framework and working within larger scale projects
* Learning from other developers and working together to resolve technical issues
* Researching and project planning for a new application
