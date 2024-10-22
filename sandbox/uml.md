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
    - registered_at: timestamp
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
    - created_at: timestamp
    - updated_at: timestamp
    - visibility: Visibility
}

class Post {
    - post_ID: int
    - topic: Topic
    - user: User
    - title: string
    - slug: string
    - content: string
    - created_at: timestamp
    - updated_at: timestamp
    - pinned_reply: Reply
    - visibility: Visibility
}

class Reply {
    - reply_ID: int
    - post: Post
    - user: User
    - content: string
    - created_at: timestamp
    - updated_at: timestamp
    - vote_up: int
    - vote_down: int
    - visibility: Visibility
}

class Visibility {
    <<enumeration>>
    PUBLIC
    MEMBERS_ONLY
    HIDDEN
    PRIVATE
}

User "1" -- "0..*" Topic
User "1" -- "0..*" Post
User "1" -- "0..*" Reply
Topic "1" --> "0..*" Topic: has child topics
Topic "1" -- "0..*" Post
Post "1" -- "0..*" Reply
Post "1" --> "0..1" Reply: has pinned reply 

%% Transaction records

class Transaction {
    - txn_ID: int
    - user: User
    - type: TransactionType
    - status: TransactionStatus
    - task: Task
    - title: string
    - amount: float
    - created_at: timestamp
    - updated_at: timestamp
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
    - title: string
    - amount: float
    - prev_amount: float
    - created_at: timestamp
    - updated_at: timestamp
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