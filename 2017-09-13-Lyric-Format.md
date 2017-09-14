---
layout: post
title:  Lyric Format
--- 
## Lyric Format Document
Referred to [wiki page](https://en.wikipedia.org/wiki/LRC_(file_format))

```
LRC (short for LyRiCs) is a computer file format that synchronizes song lyrics with an audio file, such as MP3, Vorbis or MIDI. When an audio file is played with certain music players on a computer or on modern digital audio players, the song lyrics are displayed. The lyrics file generally has the same name as the audio file, with a different filename extension. For example, song.mp3 and song.lrc. The LRC format is text-based and similar to subtitle files.

Contents 
1	Formats
1.1	Simple format
1.2	Simple format extended
1.3	Enhanced format
2	Supporting
2.1	Hardware
2.2	Software
3	See also
4	References

Formats
Simple format
Simple LRC format was introduced by Kuo (Djohan) Shiang-shiang's Lyrics Displayer. It was one of the first programs, if not the first, that attempted to simulate Karaoke performance[citation needed]. It usually displays a whole line of lyrics, but it is possible to display a word at a time, such as one would see in modern Karaoke machines, by creating a time tag for each word rather than each line.

The Line Time Tags are in the format [mm:ss.xx] where mm is minutes, ss is seconds and xx is hundredths of a second.

Normal example:
[00:12.00]Line 1 lyrics
[00:17.20]Line 2 lyrics
[00:21.10]Line 3 lyrics
...
[mm:ss.xx]last lyrics line

ID Tags may appear before the lyrics,[1] although some players may not recognize or simply ignore this.[citation needed]

[ar:Lyrics artist]
[al:Album where the song is from]
[ti:Lyrics (song) title]
[au:Creator of the Songtext]
[length:How long the song is]
[by:Creator of the LRC file]
[offset:+/- Overall timestamp adjustment in milliseconds, + shifts time up, - shifts down]
 
[re:The player or editor that created the LRC file]
[ve:version of program]

Example with ID tags:
[ar:Chubby Checker oppure  Beatles, The]
[al:Hits Of The 60's - Vol. 2 – Oldies]
[ti:Let's Twist Again]
[au:Written by Kal Mann / Dave Appell, 1961]
[length: 2:23]

[00:12.00]Naku Penda Piya-Naku Taka Piya-Mpenziwe
[00:15.30]Some more lyrics ...
...

Simple format extended
Available only in Walaoke from Walasoft. The ability to change and specify the gender of the lyrics by using M: Male, F: Female, D: Duet.

Example:
[00:12.00]Line 1 lyrics
[00:17.20]F: Line 2 lyrics
[00:21.10]M: Line 3 lyrics
[00:24.00]Line 4 lyrics
[00:28.25]D: Line 5 lyrics
[00:29.02]Line 6 lyrics
Let's say we use blue for male, red for female and pink for Duet. Line 1 using the default color (blue) when no tag is found. Line 2 lyrics start with red when F: is found. Line 3 lyrics start with blue when M: is found. Line 4 lyrics stays blue when no tag is found. Line 5 lyrics start with pink when D: is found. Line 6 lyrics stays pink when no tag is found.

Enhanced format
Enhanced LRC format is an extension of Simple LRC Format developed by the designer of A2 Media Player. It adds an extra Word Time Tag in the format: <mm:ss.xx>.

Example of an Enhanced LRC file:

[mm:ss.xx] <mm:ss.xx> line 1 word 1 <mm:ss.xx> line 1 word 2 <mm:ss.xx> ... line 1 last word <mm:ss.xx>
[mm:ss.xx] <mm:ss.xx> line 2 word 1 <mm:ss.xx> line 2 word 2 <mm:ss.xx> ... line 2 last word <mm:ss.xx>
...
[mm:ss.xx] <mm:ss.xx> last line word 1 <mm:ss.xx> last line word 2 <mm:ss.xx> ...  last line last word <mm:ss.xx>
```


## Lyric Format Example

```
[00:-1.00]
[by:163]
[re:LyricsX]
[ar:Brian Eno]
[al:Wall Street: Money Never Sleeps (Music from the Motion Picture)]
[ve:1]
[ti:Life is Long]
[00:00.000]
[00:02.100]
[00:16.700]Ev'rybody says that the living is easy【每个人都说生活很容易】
[00:22.460]I can barely see 'cause my head's in the way【我不知道，因为我头脑一片混乱】
[00:28.090]Tigers walk behind me- they are to remind me that【老虎在我的身后，他们提醒我：我已经迷失在了路上】
[00:34.350]I'm lost- but I'm not afraid【但是我不害怕】
[00:38.950]
[00:39.750]Soul to soul- A kiss and a sigh【灵魂与灵魂之间 一个吻一声叹息】
[00:45.270]Sawed in half- by the passage of time【被时间的长廊切成两半】
[00:50.460]Halfway home- from a window you see【再回家的路途上， 从窗户你看到了锁链和铁栏】
[00:56.840]Chains and bars- but I am still free【但我仍然是自由的】
[01:02.400]
[01:14.080]People on the outside- I remember sweet times【在外面的人们-我仍记得那美好的时光】
[01:20.200]This old rose- is always in bloom【那已盛开的玫瑰，将永远的绽放】
[01:25.770]Ev'ryone is happy- to be a baby daddy【每个人都非常开心，去做一个宝贝的爸爸】
[01:31.620]Stone love- with nothing to lose【纯粹的爱- 你一无所失】
[01:36.650]
[01:37.430]Life is long- if you give it way【如果你献出你的生命，那生命将是漫长的】
[01:42.750]So stay, don't go- 'cause I'm fading away【留下来，不要走，因为我正慢慢的消散】
[01:48.150]Soul to soul- between you and me【你与我，灵魂与灵魂之间】
[01:54.440]Chain me down- but I am still free【将我束缚，但我仍然还是自由】
[01:59.850]
[02:12.440]Now I can say- those three little words【现在我可以向你倾诉我的情衷】
[02:18.230]And ev'ry day- I'm dreaming a world【每天，我都在梦想着一个世界】
[02:24.270]Soul to soul- a kiss and a sigh【灵魂相对着灵魂，一个吻 一声叹息】
[02:30.170]Holding back- the waters outside.【阻挡住外面世界的大】
[02:35.080]
[02:35.600]Life is long- if you give it way【如果你献出你的生命 ，它如此漫长】
[02:41.140]So stay, don't go- 'cause I'm fading away【所以留下，不要离开，因为我就要消亡】
[02:47.000]Soul to soul- between you and me【你与我，灵魂之间】
[02:52.760]Chain me down- but I am still free【将我束缚，而我依然自由】
[02:58.630]
[03:00.100]
[03:01.400]
[03:02.650]
```


