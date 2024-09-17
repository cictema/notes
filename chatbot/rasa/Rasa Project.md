
## Init
---
```bash
rasa init
```

## Folder Structure and Assets
- `data/nlu.yml`: Contains the training data in the form of **intents** and **entities**. **Intents** are used to classify individual user messages.
- `data/stories.yml`: Contains stories/conversations between the bot and a user.
- `data/rules.yml`: Contains rules that handle small conversations at any point.
- `config.yml`: Contains the pipeline that the bot will use to interpret the message.
- `domain.yml`: Contains all the intents and their corresponding responses.

## Process

### Define Intent
```yaml
version: "3.1"
nlu:
- intent: greet
	examples: |
	- hey
	- hello
	- hi
	- hello there
	- good morning
	- good evening
	- moin
	- hey there
	- let's go
	- hey dude
	- goodmorning
	- goodevening
	- good afternoon

- intent: goodbye
	examples: |
	- cu
	- good by
	- cee you later
	- good night
	- bye
	- goodbye
	- have a nice day
	- see you around
	- bye bye
	- see you later

- intent: affirm
	examples: |
	- yes
	- y
	- indeed
	- of course
	- that sounds good
	- correct

- intent: deny
	examples: |
	- no
	- n
	- never
	- I don't think so
	- don't like that
	- no way
	- not really

- intent: mood_great
	examples: |
	- perfect
	- great
	- amazing
	- feeling like a king
	- wonderful
	- I am feeling very good
	- I am great
	- I am amazing
	- I am going to save the world
	- super stoked
	- extremely good
	- so so perfect
	- so good
	- so perfect

- intent: mood_unhappy
	examples: |
	- my day was horrible
	- I am sad
	- I don't feel very well
	- I am disappointed
	- super sad
	- I'm so sad
	- sad
	- very sad
	- unhappy
	- not good
	- not very good
	- extremly sad
	- so saad
	- so sad

- intent: bot_challenge
	examples: |
	- are you a bot?
	- are you a human?
	- am I talking to a bot?
	- am I talking to a human?

- intenet: angry
	examples: |
	- I'm angry right now
	- Things don't work out for me
	- I could punch a wall
	- I'm so annoyed
	- This is not working out
	- I want to rage so bad
	- My rage is rising
	- Why do things have to happen this way
	- I can't hold it in any longer
	- I am so frustrated
```

### Define Stories
```yaml
version: "3.1"
stories:
- story: happy path
	steps:
	- intent: greet
	- action: utter_greet
	- intent: mood_great
	- action: utter_happy

- story: sad path 1
	steps:
	- intent: greet
	- action: utter_greet
	- intent: mood_unhappy
	- action: utter_cheer_up
	- action: utter_did_that_help
	- intent: affirm
	- action: utter_happy

- story: sad path 2
	steps:
	- intent: greet
	- action: utter_greet
	- intent: mood_unhappy
	- action: utter_cheer_up
	- action: utter_did_that_help
	- intent: deny
	- action: utter_goodbye

- story: angry path 1
	steps:
	- intent: greet
	- action: utter_greet
	- intent: angry
	- action: utter_calm_down
	- action: utter_did_that_help
	- intent: affirm
	- action: utter_happy
	- action: utter_goodbye
```

## Defining Rules
---
Some messages do not require full-length conversations. 

They can be resolved by always following the same path using a single response. 
These are defined as **rules**.

```yaml
version: "3.1"
rules:

- rule: Say goodbye anytime the user says goodbye
	steps:
	- intent: goodbye
	- action: utter_goodbye

- rule: Say 'I am a bot' anytime the user challenges
	steps:
	- intent: bot_challenge
	- action: utter_iamabot

- rule: Say 'hi' anytime the user the greets the bot
	steps:
	- intent: greet
	- action: utter_greet

```


## Train Model
---
```
rasa train
```

Trains model and puts it n ./models/

## Start Model Inside Shell
---
```bash
rasa shell
```

## Interactive Training
```bash
rasa interactive
```

Talk to bot about utterances and intents and improve data set.

## Check Data for Inconsistency:
```bash
rasa data validate
```

## Train Again 
```bash
rasa train
```

## Visualize Dataset
```
rasa visualize
```

## Run Rasa
```bash
rasa run -m models --enable-api
```