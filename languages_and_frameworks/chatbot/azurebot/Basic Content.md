it 1. Bot Framework:
    1. Channel:
        1. FB Messenger, Telegram, Web, etc
        2. User -> Channel -> Bot
        3. Multiple Channels
    2. Components:
        1. SDK
        2. Framework Emulator
        3. Azure bot Service
            1. Glue between bot and user
    3. Channel:
        1. Produces Channel Json
        2. Azure bot Service
            1. creates ActivityJson and send to bot
    4. Activity:
        1. Every interaction generates an activity
        2. Several types:
            1. ActivityTypes:
                1. Message
                2. ContactRelationUpdate
                3. ConversationUpdate
                4. DeleteUserData
                5. EndOfConversation
                6. Event
                7. InstallationUpdate:
                    1. Installation in Teams or Slack
                8. Invoke
                9. MessageReaction
                    1. Emote reaction in FB Messenger
                10. Typing
                    1. So bot know conversation is not over
                11. Suggestion
                12. Trace
                13. Handoff
    5. AcitivityObject:
        1. Different Channel = Different IDs
            1. ID is unique to both user in channels
        2. Channel Account:
            1. Recipient
            2. From
        3. Conversation Account
            1. Each conversation has a differnet ID
        4. Channel Data:
            1. Native functionaltiy for current channel
        5. Value Field:
            1. Text, image, etc
            2. Text
    6. Turn:
        1. User incoming activity and what the bot sends back
        2. Full Payload = TurnContext
            1. Activity
            2. Middleware
            3. Additional Functionality
    7. EchoBot:
        1. EchoBot:ActivityHandler:
            1. OnTurnAsync:
                1. Handles all turns
            2. OnMessageActivityAsync
                1. turnContext.SendActivityAsync
            3. OnMembersAddedAsync
            4. MessageFactory:
                1. Carousel
                2. Attachments
                3. ContentUrl
                4. SuggestedActions
    8. BotController:
        1. Adapter
        2. Bot
        3. PostAsync
        4. Route: api/messages
        5. IBot bot
            1. Dependency Injection
                1. Starup.cs
                    1. IServiceCollection services
                    2. AddMvc : WebAPI project
                    3. AddSingleton: Bot Framework Adapter
                    4. AddTransient: Ibot, Echobot
    9. ConversationState:
        1. Levels of State:
            1. StorageLayer:
                1. In memory
                2. Azure Blob Storage
                3. Azure Cosmos DB
            2. StateManagementLayer:
                1. Bot State Class
                    1. User State
                        1. User
                        2. Channel
                    2. Conversation State
                        1. Conversation ID
                        2. Channel
                    3. Private Conversation State
                        1. ConversationID
                        2. USer
                        3. Channel
                2. StateProperty:
                    1. StatePropertyAccessor
                3. Models:
                    1. UserProfile
                4. Services:
                    1. StateService:
                        1. UserState
                        2. UserProfileId
                        3. IStatePropertyAccessor<UserProfile> UserProfileAccessor
                            1. Pull, push and delete data from property in state bucket
                        4. Constructor StateService:
                            1. Set UserState
                            2. InitializeAccessors
                                1. UserProfileAccessor =UserState.CreateProperty<UserProfile>(UserProfileId)
                            
                        5. Inject StateService in EchoBot.cs
                        6. Set value for _stateService in constructor
                5. Dialog
                    1. Dialog Pieces
                        1. DialogSet
                        2. DialogContext
                        3. DialogResult
                    2. DialogType
                        1. Prompt
                            1. 2 step
                                1. ask user for info
                                2. evaluate response
                            2. Subtypes:
                                1. Text
                                2. number
                                3. datetime
                                4. confirm
                                5. choice
                                6. attachment
                            3. Prompt Options
                                1. Retry prompt
                            4. Choices:
                                1. chocolate vanilla
                            5. Validations
                                1. customvalidtors
                        2. Waterfall
                            1. Series of interactions
                                1. guide user through series of tasks
                                2. waterfall + prompt
                                3. StepContext
                        3. Component
                            1. Reusable Components