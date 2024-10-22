```mermaid
%%{init:{'theme':'dark'}}%%
classDiagram

%% Users & groups

class User {
    - user_ID: int
    - group: Group
    - email: string
    - hashed_password: string
    - salt: string
    - verification_token: string
    - registered_at: DateTime
    - timezone: UserTimezone
    - currency: UserCurrency
}

class Group {
    - group_ID: int
    - name: string
}

class Capability {
    - name: string
}

class GroupCapability {
    - group_ID: int
    - capability: string
}

class UserTimezone {
    <<enumeration>>
    UTC
    Asia/Kuala_Lumpur
    ..etc
}

class UserCurrency {
    <<enumeration>>
    MYR
    USD
    ..etc
}

User "1..*" -- "1" Group
Group "1" -- "0..*" GroupCapability
Capability "1" -- "0..*" GroupCapability
User ..> UserTimezone
User ..> UserCurrency

%% Forum, for knowledge base & customer support

class Topic {
    - topic_ID: int
    - parent_topic: Topic
    - user: User
    - title: string
    - slug: string
    - description: string
    - created_at: DateTime
    - updated_at: DateTime
    - visibility: Visibility
}

class Post {
    - post_ID: int
    - topic: Topic
    - user: User
    - title: string
    - slug: string
    - content: string
    - created_at: DateTime
    - updated_at: DateTime
    - pinned_reply: Reply
    - visibility: Visibility
}

class Reply {
    - reply_ID: int
    - post: Post
    - user: User
    - content: string
    - created_at: DateTime
    - updated_at: DateTime
    - vote_up: int
    - vote_down: int
    - visibility: Visibility
}

class Vote {
    - vote_ID: int
    - type: VoteType
    - object_type: ObjectType
    - object_ID: int
    - user: User
    - added_at: DateTime 
}

class VoteType {
    <<enumeration>>
    UP
    DOWN
}

class Visibility {
    <<enumeration>>
    PUBLIC
    MEMBERS_ONLY
    HIDDEN
    PRIVATE
}

class Log {
    - log_ID: int
    - action: string
    - user: User
    - added_at: DateTime
    - object_type: ObjectType
    - object_ID: int
    - metadata: string 
}

class ObjectType {
    <<enumeration>>
    TOPIC 
    POST 
    REPLY
}

User "1" -- "0..*" Topic
User "1" -- "0..*" Post
User "1" -- "0..*" Reply
Topic "1" --> "0..*" Topic: has child topics
Topic "1" -- "0..*" Post
Post "1" -- "0..*" Reply
Post "1" --> "0..1" Reply: has pinned reply 
Topic "1" -- "0..*" Log
Post "1" -- "0..*" Log
Reply "1" -- "0..*" Log
User "1" -- "0..*" Log
Post "1" -- "0..*" Vote
Reply "1" -- "0..*" Vote
User "1" -- "0..*" Vote
Vote ..> VoteType
Vote ..> ObjectType
Log ..> ObjectType

%% Transaction records

class Transaction {
    - txn_ID: int
    - user: User
    - type: TransactionType
    - status: TransactionStatus
    - task: Task
    - title: string
    - amount: float
    - date: Date
    - created_at: DateTime
    - updated_at: DateTime
    - remark: string
}

class TransactionType {
    <<enumeration>>
    IN
    OUT
    SAVING
}

class TransactionStatus {
    <<enumeration>>
    COMPLETED
    CANCELLED
}

Transaction ..> TransactionType
Transaction ..> TransactionStatus
Transaction "0..*" -- "0..1" Task
User "1" -- "0..*" Transaction

%% Task (Todo list / Scheduled transaction)

class Task {
    - task_ID: int
    - user: User
    - type: TaskType
    - repeat: TaskRepeat
    - repeat_until: Date
    - title: string
    - amount: float
    - prev_amount: float
    - date: Date
    - created_at: DateTime
    - updated_at: DateTime
}

class TaskType {
    <<enumeration>>
    IN
    OUT
    SAVING
    INSTALLMENT
}

class TaskRepeat {
    <<enumeration>>
    NONE
    MONTHLY
}

Task ..> TaskType
Task ..> TaskRepeat
User "1" -- "0..*" Task
```