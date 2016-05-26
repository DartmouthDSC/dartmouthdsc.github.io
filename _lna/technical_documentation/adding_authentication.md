---
title: Adding Authentication
---
Instructions for adding Dartmouth web authentication using Devise and the Omniauth-cas gem to a Hydra Head. 

## Basic Instructions
Followed the directions from the Devise wiki page on how to implement OmniAuth using Devise: 
https://github.com/plataformatec/devise/wiki/OmniAuth:-Overview

The omniauth log-in link was integrated by overwritting the following blacklight template `app/views/_user_util_links.html.erb`.

Because we are only using omniauth and aren't using database authenticatable we need to follow optional instructions to create sessions routes, add session controller and add new_session_path method to the application controller. The sessions controller for our implementation rewrites the sign_in method to point at the omniauth log-in link. 

I had trouble adding SSL cert path using the ca_path parameter. To add ssl certs had to follow the instructions here: https://github.com/intridea/omniauth/wiki/Setting-up-SSL-certificate-locations-in-Linux

### Some Things to Note
- Our implementation is different because we give users the options to log-in but they don't have to be redirected to another page where they need to provide more information or edit the information given. 
- Our implementation also redirects users to a logout page and then the user has a link back to the application. 
- If we no longer wanted to use Devise, we could use the OmniAuth gem with a cas specification. This would require
more customization of other gems, such as Blacklight.


## Examples
Used this institution's implementation of Devise with the Omniauth-ldap gem in a Hydra Head as an example:
https://github.com/nulib/images

Used this implementation of Devise only using Omniauth for authentication. Has an example of a simple session controller.
https://github.com/pantulis/devise-omniauth-only-twitter
