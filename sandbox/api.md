```mermaid
%%{init:{'theme':'dark'}}%%
classDiagram
direction LR

class App {
    <<Application>>
}

class User["/user"] {
    <<Resource>>
    get(offset, limit)
    post(email, password)
}

class User_ID["/{user_ID}"] {
    <<Resource>>
    get(user_ID)
    post(user_ID, password, timezone, currency, group_ID)
    delete(user_ID, password)
}

class User_login["/login"] {
    <<Resource>>
    post(email, password)
}

class User_logout["/logout"] {
    <<Resource>>
    post(user_ID)
}

class User_verify["/verify"] {
    <<Resource>>
    post(user_ID, verification_token)
}

class User_recover["/recover"] {
    <<Resource>>
    post(email)
}

App --> User
User --> User_ID
User --> User_login
User --> User_logout
User --> User_verify
User --> User_recover

class Forum["/forum"] {
    <<Resource>>
}

class Topic["/topic"] {
    <<Resource>>
    get(offset, limit)
    post(title, description)
}

class Topic_ID["/{topic_ID}"] {
    <<Resource>>
    get(topic_ID)
    put(topic_ID, title, description)
    delete(topic_ID)
}

class Post["/post"] {
    <<Resource>>
    get(topic_ID, user_ID, offset, limit)
    post(topic_ID, title, content)
}

class Post_ID["/{post_ID}"] {
    <<Resource>>
    get(post_ID)
    put(post_ID, title, content)
    delete(post_ID)
}

class Post_pin["/pin"] {
    <<Resource>>
    post(post_ID, reply_ID)
}

class Post_vote["/vote"] {
    <<Resource>>
    get(post_ID, offset, limit)
    post(post_ID, type)
}

class Reply["/reply"] {
    <<Resource>>
    get(post_ID, user_ID, offset, limit)
    post(post_ID, content)
}

class Reply_ID["/{reply_ID}"] {
    <<Resource>>
    get(reply_ID)
    put(reply_ID, content)
    delete(reply_ID)
}

class Reply_vote["/vote"] {
    <<Resource>>
    get(reply_ID, offset, limit)
    post(reply_ID, type)
}

App --> Forum
Forum --> Topic
Topic --> Topic_ID
Forum --> Post
Post --> Post_ID
Post_ID --> Post_pin
Post_ID --> Post_vote
Forum --> Reply
Reply --> Reply_ID
Reply_ID --> Reply_vote 
```