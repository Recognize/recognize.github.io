# Recognize Api Summary
The Recognize api provides the following: 
+ Basic access to user profile information.
+ Ability to view a users recognitions
+ Send recognitions on behalf of a user
+ Approve (+1 a recognition), or "unapprove" (removing a +1 from a recognition) a recognition.

Users authenticate access for each application using the 2-legged OAuth flow.

## Base Endpoint
http://recognizeapp.com/api/v1/

## Endpoint overview
    GET /status                                  # Check authentication status
    GET /recognitions                            # Get all recognitions for your company
    GET /recognitions?email=mary@microsoft.com   # Get all recognitions for a user
    GET /recognitions/new                        # Returns a url to fetch html for send recognition form
    POST /recognitions/:slug/approvals           # +1(approve) a recognition
    DELETE /recognitions/:slug/approvals         # Remove the +1 for a recognition
    GET /badge/:badge_id                         # Get more detailed information on a badge
    
### Authentication Example in Ruby

    require 'oauth2'
    redirect_uri = "http://yoursite.com/auth/recognize/callback"
    client = OAuth2::Client.new(<APPLICATION_ID>, <APPLICATION_SECRET>, site: "https://recognizeapp.com")
    client.auth_code.authorize_url(:redirect_uri => redirect_uri)

Visit authorize url in a browser and follow the flow until you get sent to your callback url.
Find authorization code in url for next step.

    token = client.auth_code.get_token(<authorization_code>, redirect_uri: redirect_uri).

Visit:

    https://recognizeapp.com/api/v1/ping?access_token=<token>

Access token can also be sent as a header:

    Authorization: Bearer <token>

### Users (Path: /users)

    /users

    {
     "users": [
      {
      "id": 2,
      "email": "alex@recognizeapp.com",
      "first_name": "Alex",
      "last_name": "Grande",
      "full_name": "Alex Grande",
      "company_name": "Recognizeapp",
      "yammer_id": "1489441844",
      "avatar_thumb_url": "\/uploads\/development\/avatar_attachment\/9517\/file\/thumb_avatar.jpg"
      }
      ]
     }
### Recognitions (Path: /recognitions)

    /recognitions

    {
    "recognitions": [
    {
      "id": 11367,
      "message": "For showing leadership in starting recognition.",
      "slug": "17pr8evjcp9",
      "created_at": "2014-05-20 12:45:07 -0700",
      "badge_name": "Ambassador",
      "badge_permalink": "\/uploads\/development\/badge\/32\/image\/large_thumb_ambassador.png",
      "recipients": [
        {
          "id": 11667,
          "email": "peter@ad.recognizeapp.com",
          "first_name": "Peter",
          "last_name": "Philips",
          "full_name": "Peter Philips",
          "company_name": "Ad Recognizeapp",
          "avatar_thumb_url": "\/assets\/icons\/user-default.png",
          "yammer_id": ""
        }
      ],
      "approvers": [
        
      ],
      "sender": {
        "id": 1,
        "email": "app@recognizeapp.com",
        "first_name": "Recognize",
        "last_name": "Team",
        "full_name": "Recognize Team",
        "company_name": "Recognizeapp",
        "yammer_id": "",
        "avatar_thumb_url": "\/uploads\/development\/avatar_attachment\/33\/file\/thumb_logo_180x180.png"
      },
      "permalink": "http:\/\/localhost:3000\/recognitions\/17pr8evjcp9",
      "system_recognition?": "true",
      "friendly_created_at": "about 10 hours ago",
      "approvals_count": 0
    },
    }
