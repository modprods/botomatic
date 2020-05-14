botomatic is an AWS Lambda function for running a bot in a Hubs room.
It creates an instance of chromium and joins a room. The bot moves around,
as defined in bot-recording.json, and 'speaks' using bot-recording.mp3.

[serverless](https://www.serverless.com) is recommended to deploy and
manage this program.

Install serverless plugin deps using npm:

	npm install

Then set the AWS region to deploy botomatic to in serverless.yml.
Finally, deploy the script:

	serverless deploy

We can invoke botomatic by making a HTTP GET request to the published
endpoint.

	https://$app/public/run

The room, lobby to join and other bot options are passed as URL query
parameters. The parameters are:

**host** The hostname of the hubs server. For example "hubs.example.com".

**hub_sid** The ID of the room to join. For example "SZAKcir".

**duration** Number of seconds the bot should stay in the room for.

**audio** If "true" or 1 (or other JS boolean true) bot-recording.mp3 is played by each bot. The default is false.

**slow** If JS boolean true, the upload and download throughput of the spawned chromium browser is limited, and latency is increased significantly. The default is false.

For example, using curl(1):

	curl -G \
		-d host=hubs.example.com \
		-d hub_sid=SZAKcir \
		-d duration=30
		https://$app/public/run

To read botomatic's logs, read the AWS lambda logs using serverless:

	serverless logs -f app
