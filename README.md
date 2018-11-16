# Doorman

My last apartment had one of those callbox entry systems, where when your ~~Postmates driver pulls up~~ friend comes over,
they dial your code in the callbox, which calls you, and you dial 9 to let them in. Seems simple enough.

Unfortunately though, my room mate and I only had one entry code we had to share. At first, we just set up a [Google Voice](https://voice.google.com/about) number that would forward the callbox calls to the both of us, and the first one of us to
answer would press 9, and buzz the person in. That worked well enough... for awhile.

Then one day I realized 2 things:
1. I was getting really annoyed getting a phone call everytime my room mate ~~got Jimmy John's delivered~~ invited someone over.
2. There was absolutely no security, because as long as you dialed our code, even if I was not expecting anyone, I would press
9 assuming my room mate was, and vice versa.

So I decided to just automate that process with [Twilio](https://www.twilio.com/).

![image](https://user-images.githubusercontent.com/3345162/48640095-3dc01000-e99b-11e8-9744-8f16974659ad.png)

## Setup

1. Make a Twilio Account.
2. Buy a phone number from Twilio to use with your new account ($1/month).
3. Fork this repo.
4. Edit the twiml.xml file, and replace the phone numbers (with no spaces or parentheses but with the country code i.e. +15558675309):
    1. Replace ```MY_PHONE_NUMBER``` with your phone number.
    2. Replace ```MY_TWILIO_NUMBER``` with the phone number you bought for your Twilio account.
    3. If applicable, replace ```MY_ROOMMATES_PHONE_NUMBER``` with your room mate's phone number, if not, just remove that line
5. Host your twiml.xml file somewhere.
6. Go to your Twilio console, and configure your phone number to HTTP get your hosted twiml.xml file when a call comes in.
7. Have your callbox call your Twilio number.
8. NEVER ANSWER A PHONE CALL AGAIN.

![image](https://user-images.githubusercontent.com/3345162/48639311-d6a15c00-e998-11e8-9f5d-b1a01057821a.png)

## Cost

Twiml Charges:
- $1/month for a phone number
- $0.0085/minute for an incoming phone call
- $0.0075/message for an outgoing text message

So this will run you a little less than $.02 everytime someone buzzes in. IMHO, worth it.

For more information on Twilio pricing, [go here](https://www.twilio.com/pricing).

## This file seems a little repetitive...

TwiML's [Play verb](https://www.twilio.com/docs/voice/twiml/play) documentation days that it has 2 properties: ```digits``` and ```loop```. According to the documentation:
- ```digits``` lets you specify numbers that you want dialed, and 'w' if you want it to pause for .5 seconds
- ```loop``` lets you specify the number of times you want the sequence to loop

Neither of those worked for me the way the documentation specified, so I ended up just using the Play verb to dial '9' and then the [Pause verb](https://www.twilio.com/docs/voice/twiml/pause) to pause for a second, and repeating that 10 times, just in case the call took awhile to connect to the callbox.
```
<Play digits="9"/>
<Pause length="1"/>
```
