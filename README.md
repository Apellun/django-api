# Django course project

<p>This is a Django api that provides a platform for users to create ads, comment on each other ads, and edit and delete the ads they have created. Users can also search ads by title. The app is built with Django Rest Framework and Djoser library with a custom user model.</p>

## Main viewpoints

<ins>api/ads/</ins>
<p><b>GET</b> — Displays a list of ads, 4 for each page. Everyone can view it. Everyone also can search ads by title with the query parameter "title".<br>
<b>POST</b> - Creates a new ad, which requires the title and the price. Accessible for authorized users only.</p>

<ins>api/ads/pk/</ins>
<p><b>GET</b> - Displays detailed info on the ad. Everyone can view it.<br>
<b>PUT, PATCH</b> — Updates and partially updates info on the selected ad. Only the ad's owners and admin users can change ads.<br>
<b>DELETE</b> — Deletes the selected ad. Only the ad's owners and admin users can delete ads.</p>

<ins>api/comments/</ins>
<p><b>GET</b> - Displays a list of all users' comments. Accessible for authorized users only.<br>
<b>POST</b> - Creates a comment, for that requires the text. Accessible for authorized users only. Gets the ad's that is commented upon id from the query parameter "ad".</p>

<ins>api/comments/pk/</u><br>
<p><b>GET</b> - Displays detailed info on comment. Accessible for authorized users only.<br>
<b>PUT, PATCH</b> — Updates and partially updates info on the selected comment. Only the comment's authors and admin users can change ads.<br>
<b>DELETE</b> — Deletes the selected comment. Only the comment's authors and admin users can delete comments.</p>

<ins>api/users/ and authentication endpoints</u><br>

<p>All of the main <ins>users/</ins> and authentication endpoints work exactly like they are described in Djoser documentation. More info on it here: <i>https://djoser.readthedocs.io/en/latest/base_endpoints.html</i></p>

## Challenges

### Djoser + custom user model and user manager

<p>For this project, I used Djoser for the first time. I've read the documentation and used a custom User model and User manager provided by course tutors to implement it correctly.
It took some time to get the User manager to work properly. I had the constant issue of it making a superuser through a ./manage.py command, but assigning this user the non-admin role. When I got it to create superusers properly, it started creating common users with an admin role. I figured out the solution by now, but I still feel that I do not understand the User manager function in-depth, information on the subject seems to be a bit scarce.</p>

### Different permissions for different actions in a viewset

<p>I wrote multiple permissions for one viewset for the first time. It was not particularly hard, because I was able to find a solution online. Nevertheless, it took me some time to understand how to adjust it to the project correctly.</p>

### Auto user id attach

<p>At first, the ads' endpoint "create" method has been receiving value for the author field (a foreign key — the ad owner's user id) from the body of a query, like the title and the price of the ad. At some point, I thought that it is weird to pass the id like that. I asked a senior colleague and he confirmed, that it is a weird thing to do, indeed.
I decided to replace the default "create" method of the viewset so it could get the author's id not from the body, but from the session details, automatically. My current solution for that is a hardcode, since I couldn't find a reference for a better way to do that.
I used that solution in the comments' endpoint "create" method as well, with some extra logic so it also gets the ad's id from a query parameter and the created comment gets tied to an ad automatically.</p>

### Password reset email confirmation

<p>For this part, adjusting the settings.py was easy, but understanding how the Djoser reset_password endpoints and their methods work was hard.
I found a tutorial for testing this functionality here https://saasitive.com/tutorial/django-rest-framework-reset-password/, put it into my project and adjusted the code of both the test and the api so that the test passed without mistakes. It helped me understand the functionality and become sure that I set everything correctly.
I kept the author's comments for future reference.</p>
