https://flask.palletsprojects.com/en/1.1.x/quickstart/
automated story sharing, voice recording
sms or email interactions, or phone call! like, calling them, asking a question, recording the answer
maybe just...call in person...like a nice human lol

but anyway:
phone call, question, they speak into the phone, call is recorded
audio -> text application
edit text for correctness
store audio and text

use app to record phone call
upload recording to google drive folder
cron job -> poll google drive folder for updates
if audio file uploaded, take file and run speech-to-text engine
Athena
archive text and audio



yo...what if it was like, whenever you want, you can call this number and record a story?

api to record the call

twilio
python
web server, flask?
aws

use case:

START
call comes in,
gets answered, "Hello, this is the story routing service. By using this service, you consent to having this call recorded. please enter your pin: "
-call our api to start recording the call
-take in pin
-search account database for pin
--if necessary, cross-reference phone number for account identification
-if exists:
	-return name of account owner

	"hello <account name>. press 1 to hear a prompt, press 2 to record directly, or hang up at any time"
	if 1:
	-	query random prompt
	-	make sure prompt hasnt been done already
		log prompt as asked
	-	return prompt
		"prompt: <prompt>. do you wish to use this prompt? press 1 for yes, press 0 for another prompt, or hang up at any time"
	-	if 0:
	-		REPEAT
	-	if 1:
			log prompt as chosen
			CONTINUE to 2
		else (after timeout):
			"we have received no response. goodbye."
		-	save recording
		-	analyze recording (speech-text)
		-	save to database
		-	note that no response was received for authenticated account
		-	-END
	if 2:
		"alright great, ill transfer your call to the recording service!"
	-	transfer call to skype number
	-	save call recording
	-	skype number picks up
	-	record
		"hello this is the recording service, by using this service, you consent to having this call recorded. i mean, its called the recording service. at the tone, start recording your answer: *tone*"
	-	record until call ends(this might get expensive. maybe set a hard cutoff at 10 minutes? 5 minutes? kinda defeats the purpose)
	-	save call recording
	-	run recording through speech-to-text
	-	save audio file and text file in database,
	-	check for and note any duration abnormalities
		-END

	else (after timeout):
		"we have received no response. goodbye."
	-	save recording
	-	analyze recording (speech-text)
	-	save to database
	-	note that no response was received for authenticated account
	-	-END
-else:
"sorry, that account does not exist. goodbye."	
-note that no account existed
-save recording
-analyze recording(speech-text)
-save to database
-note that authentication attempt failed
-END

END


API endpoints:


authenticate:
	-take in pin
	-search account database for pin
	--if necessary, cross-reference phone number for account identification
	-if exists:
		-return name of account owner
	else:
		-throw exception

^^^^^^^^^actually instead of this, why dont i just whitelist individual numbers with twilio? i can use an auth service as an extendible feature

select prompt:
	query random prompt
	-	make sure prompt hasnt been done already, but if its been asked before and not chosen lru principle etc (algorithm)
		log prompt as asked
	-	return prompt
	log if prompt is chosen

speech-to-text:
	take in recording
	run through speech-to-text module
	save output and unmodified input in database
	return

view/edit account information:

notify when theres a new entry

setup twilio library/learn twiml

setup speech to text

run project server on aws



WOW THIS GOT OFF TRACK FROM ITS ORIGINAL PURPOSE
none of this is mvp. mvp is the following:

very basic auth web portal to upload audio recording
when upload is triggered, run recording through speech-to-text
save text and audio files

EVERYTHING ELSE IS EXTRA UNTIL THIS IS DONE

flask server