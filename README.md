# AVPlayerRateBug
Demonstrating the bug in AVPlayer's rate reporting on macOS

## Synopsis

This project exists to demonstrait that there is a problem with the rate's reported with AVPlayer in macOS.  One method from Apple's sample code, `- (void)rightMouseDown:(nonnull NSEvent *)theEvent` has been modified for this bug demonstration.  This method now prints 2 different kinds of rate information to the xcode console.

## Code Example
Below is the only code modification from Apple's [AVMovieEditor Sample Project.](https://developer.apple.com/library/content/samplecode/AVMovieEditor/Introduction/Intro.html)  I also added the import line `#import <Appkit/Appkit.h>` in AAPLMovieMutator.m because the sample code would not run without this.

    - (void)rightMouseDown:(nonnull NSEvent *)theEvent {
        NSLog(@"RequestedPlaybackRate:%f",self.playerView.player.rate);
        NSLog(@"ActualPlaybackRate:%f",CMTimebaseGetRate(self.playerView.player.currentItem.timebase));
    }

## Test

To experience this bug download and run Apple's AVMovieEditor as modified in this repository.  Simply open any local movie file and right click anywhere on the thumbnail image filled "movie timeline" appearing along the bottom of the apps window.  This should print 2 different kind of rate reports to the xcode console.  Both values should be "0.0" as the video is not playing.  Now press the "L" key on the keyboard.  Again right click the mouse on the "movie timeline".  This should again print 2 kinds of rate reports and both values will now be "1.0" indicating video is playing at 1x of it's intended speed.  A second pressing of the "L" key will increase the playback speed to 2x, this should be visually aparent watching the video.  Again for a 3rd time, right click the mouse. 2 more rate values will be printed to the console, but instead of being "2.0" they are both "0.0".  Continuing to press the "L" key will cycle through speeds 2x, 5x, 10x, 30x & 60x, but both the `AVPlayer.rate` and `AVplayerItem.timebase.rate` show all of these speeds as a rate of "0.0".  Similarly incorrect rates can be observed using the "J" key and playing the video in reverse at high speeds.

## Motivation

My motivation for creating this project is that I am trying to create playback speed indicator for a custom movie editing UI.  I need my user to be able to playback video footage at high speed and quickly and easily see what the speed is.  I've never created a project like this nor officially submitted a bug report anywhere, any feedback would be greatly appreciated.
