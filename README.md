# qt-solutions
Fork of qtservice to make it work again on macOS

## Summary
This is my fork of the aging qtsolutions package.  In particular, I am heavily dependent on the qtservice tool, which has broken in a recent release of macOS, and I'm using this fork to experiment with some changes to the qtservice sources to make it work again.

## Apple and macOS
Since this is my fork, excuse me while I feel free to rant on Apple and its policies with macOS.

It's pretty obvious that Apple looks at OS X/macOS merely as a platform for doing mobile app development, a necessary evil since there's no better way to allow third-party developers to develop for their mobile hardware.  Anybody doing desktop-only development for this operating system usually gets screwed in some fashion with each new minor release, and they've cleverly trained their rabid customers to blame the software, not the operating system, when things break.

The current idiocy not only deprecates the fairly elegant solution of AuthorizationExecuteWithPrivileges() (since 10.7), but now locks out write access to /Library/Preferences.  Unfortunately, qtservice uses the QSettings::SystemScope when creating its configuration settings, and under OS X/macOS with Qt 5.11.1, this maps directly to /Library/Preferences.  Of course, this now fails, and the Qt-based service cannot be installed via the QtServiceController's install() method.

As I understand it, the alternative to AuthorizationExecuteWithPrivileges() is now some kind of separate "helper" process, created with SMJobBless, that runs as root at all times, with which your non-root process (i.e., the one using QtServiceController) must communicate via XPC to instruct it to perform root-based actions.  In comparison to the simplicty of the now-deprecated AuthorizationExecuteWithPrivileges(), this is a ludicrous solution developed by (what can only be regarded by my 35 years of software engineering experience as) amateurs.

When you consider Apple's constantly hostile decisions toward third-party development, coupled with Apple's choice to thumb its nose at industry standards and deprecate OpenGL in favor of Metal instead of Vulcan, it's hardly a wonder that macOS is starting to bleed third-party support.  I've about had my fill as well.

To be fair, I bear no love for Microsoft either (especially after Windows 8 and now Windows 10), but qtservice runs correctly without modifications on versions of Windows BACK THROUGH 2001!!  They have always taken backward compatibility and third-party software support seriously. (Admittedly, Microsoft is moving toward the Apple model, so that last statement may be valid only for a limited time.) Apple simply does not, and hasn't in the entire time I've had to support OS 9, OS X and, now, macOS.

&lt;/rant&gt;
