# Use example

```js
const { getVideo } = require('@fabricio-191/youtube');
const fs = require('fs');

getVideo('https://www.youtube.com/watch?v=zWRvVbyQjQY&list=PLDS0dpumEOi0pu_0pCGqvcaRkxg-o1gqg&index=95')
    .then(async video => {
		//By default it downloads only the audio, in the best possible quality

        let buffer = await video.download(true); //download as buffer
        fs.writeFileSync('./audio.mp3', buffer);
        //the best audio is probably not going to be a mp3

        let stream = await video.download(); //download as stream
        stream.pipe(fs.createWriteStream('./audio2.mp3'));
    })
```

# Chosing a format

To choose a format you should see `videoData.formats`, it's an object like this:

```js
{
    videoAndAudio: [...],
    video: [...],
    audio: [...]
}
```

All formats are ordered in such a way that the first is the best

A format with audio and video is like this  
If the format is only audio, the `video` property will not be present, and vice versa.
```js
{
	itag: 18,
	type: 'mp4',
	size: '141 MB',
	byteLength: 147959020,
	quality: 'medium',
	download: [Function: download],
	expiresAt: 1610096359465,
	meta: {
		bitrate: 366578,
		averageBitrate: 366577,
		contentLength: '1183672162',
		approxDurationMs: '25831874',
		lastModified: '1609600959271530',
		projectionType: 'RECTANGULAR'
	},
	audio: {
		codec: 'mp4a.40.2',
		simpleCodec: 'MP4a',
		channels: 2,
		quality: 'LOW',
		sampleRate: 44100
	},
	video: {
		codec: 'avc1.42001E',
		simpleCodec: 'AVC1',
		width: 640,
		height: 360,
		label: '360p',
		fps: 30
	}
}
```
Once you get the format you want you call the download method

# Example
I want an Opus audio to reproduce it in Discord

```js
getVideo('https://www.youtube.com/watch?v=zWRvVbyQjQY&list=PLDS0dpumEOi0pu_0pCGqvcaRkxg-o1gqg&index=95')
    .then(async data => {
		let format = data.formats.audio.find(format => format.audio.simpleCodec === 'Opus') //the first that use opus
		/*
		Remember, the formats are ordered from best to worst
		So, is the best with Opus as codec
		*/

		let stream = await format.download();
		//voiceConnection, from Discord.js
        voiceConnection.play(stream);
    })
```

Here is the list of simplified codecs, lower index = better

```js
[
	//video
	'AV01',
	'H.264',
	'VP9',
	'VP8',
	'MPEG-4',
	'H.283', 
	'AVC1',
	'MP4v',
	
	//audio
	'FLAC',
	'Opus',
	'AAC',
	'Vorbis',
	'MP3',
	'MP4a'
];
```

# Example 2

```js
//something like 'C:/Users/Fabricio/Downloads'
let downloadsPath = `${process.env.HOMEDRIVE}${process.env.HOMEPATH}/Downloads`;

getVideo('https://www.youtube.com/watch?v=zWRvVbyQjQY&list=PLDS0dpumEOi0pu_0pCGqvcaRkxg-o1gqg&index=95')
	.then(async video => {
		let format = video.formats.video
			.reduce((best, format) => {
				if(
					format.byteLength < best.byteLength && (  //less size
						format.height >= best.height ||  //and more or equal resolution
						format.fps >= best.fps  //or more or equal fps
					)
				) best = format;

				return best;
			}, video.formats.video[0]);

		let buffer = await format.download(true);
		fs.writeFileSync(`${downloadsPath}/${video.name}.${format.type}`, buffer);
	});
```