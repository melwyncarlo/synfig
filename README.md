Synfig Studio
=============

About
-----

Synfig Studio is a free and open-source 2D animation software, designed as
powerful industrial-strength solution for creating film-quality animation using
a vector and bitmap artwork. It eliminates the need to create animation
frame-by frame, allowing you to produce 2D animation of a higher quality with
fewer people and resources. Synfig Studio is available for Windows, Linux and
MacOS.

https://synfig.org/

[![Build Status](https://travis-ci.com/synfig/synfig.svg?branch=master)](https://app.travis-ci.com/synfig/synfig)
[![GitHub Actions](https://github.com/synfig/synfig/workflows/Synfig%20CI/badge.svg?branch=master)](https://github.com/synfig/synfig/actions)
[![Windows Build status](https://img.shields.io/appveyor/ci/synfig/synfig/master.svg?label=Windows%20build)](https://ci.appveyor.com/project/Synfig/synfig/branch/master)

Installing
----------

<br>

### My personal (Melwyn Francis Carlo's) building Synfig from source steps on Windows

<br>

<p>
                        This information is about building <a href="https://www.synfig.org/">Synfig</a> directly from its <a href="https://github.com/synfig/synfig">source code</a>.<br/>
                        General information regarding this has been posted by Synfig at <a href="https://synfig-docs-dev.readthedocs.io/en/latest/building/Building%20Synfig.html">this link</a>.<br/>
                        But, that does not suffice, as I have encountered <i>a number of problems</i> while building it on my Windows 10 computer system.
                        So I put in the effort and figured out a way to successfully build and install it. The following steps will hopefully make your life easier.
                    </p>
                    <p>
                        <ul>
                            <li>Before you begin, you will first need to download the <code>MSYS2</code> application from <a href="https://www.msys2.org/">this website</a>,
                            and follow the instructions as given on that website.</li>
                            <li>Next, launch the <code>MSYS2 MINGW64</code> application (it's a special command window). If you have a 32-bit computer (most have 64-bit these days), then you might have MINGW32 instead; so, launch that application. Furthermore, (if you have a 32-bit computer) any references to <i>64-bit</i> in any commands that follow must be replaced with its <i>32-bit</i>-equivalent, wherever required.</li>
                            <li>Next, download the source code zip file from <a href="https://github.com/synfig/synfig/releases">this link</a>.
                            Move the zip file to a location of your choice, and then unzip (extract here) the zip file.
                            Enter that folder, (e.g.: synfig-1.5.3) and copy the folder path (<kbd><kbd>Alt</kbd> + <kbd>d</kbd></kbd> followed by <kbd><kbd>Ctrl</kbd> + <kbd>c</kbd></kbd>).</li>
                            <li>In the command window, type <kbd>cd</kbd> following by a space, and then paste the copied path and then hit enter.<br/>
                            For instructional purpose, let's assume the folder we're currently in is: <code>synfig-1.5.3</code></li>
                            <li>
                                Replace the MLT library version in various files from <code>7.2.0</code> to the latest version. (in my case, it was <code>7.34.1</code>)
                                This version can be obtained by checking out <a href="https://github.com/mltframework/mlt/releases">this link</a>.
                                <br/><br/>
                                <table>
                                    <thead>
                                        <th>File path</th>
                                        <th>&nbsp;&nbsp;Line number</th>
                                    </thead>
                                    <tbody>
                                        <tr>
                                            <td>synfig-1.5.3/autobuild/msys2/build_mlt.sh</td>
                                            <td>&nbsp;7</td>
                                        </tr>
                                        <tr>
                                            <td>synfig-1.5.3/2-build-cmake.sh</td>
                                            <td>&nbsp;13</td>
                                        </tr>
                                        <tr>
                                            <td>synfig-1.5.3/autobuild/synfigstudio-release.sh</td>
                                            <td>&nbsp;45</td>
                                        </tr>
                                        <tr>
                                            <td>synfig-1.5.3/cmake/InstallMSYS2.cmake</td>
                                            <td>&nbsp;99</td>
                                        </tr>
                                        <tr>
                                            <td>synfig-1.5.3/cmake/InstallMSYS2.cmake</td>
                                            <td>&nbsp;109</td>
                                        </tr>
                                        <tr>
                                            <td>synfig-1.5.3/autobuild/build.sh</td>
                                            <td>&nbsp;93</td>
                                        </tr>
                                    </tbody>
                                </table>
                                <br/>
                            </li>
<li>Open <code>synfig-1.5.3/autobuild/msys2/build_mlt.sh</code>. On <u>line number 28</u> (cmake -G"Ninja"...), append the following before the double-dots ('..'):<br/><code>-DJACK_LIBRARY=/mingw64/lib/libjack.dll.a -DCMAKE_PREFIX_PATH=/mingw64</code></li>
                            <li>Using the old Movit library in Synfig causes build error, so let's build it from source.</li>
                            <li>Download the latest Movit library file from <a href="https://movit.sesse.net/">movit.sesse.net</a> (the latest version is 1.7.2 in tar.gz format as of December 2025). Unzip the zip file in a folder of your choice, and then enter that folder. For instructional purpose, the folder name is: <code>movit-1.7.2</code>. Copy this folder path.</li>
                            <li>
                                In the command window:
                                <ol>
                                    <li>type <kbd>cd</kbd> following by a space, and then paste the copied path and then hit enter</li>
                                    <li>type <kbd>pacman -S mingw-w64-x86_64-eigen3</kbd> and then hit enter</li>
                                    <li>type <kbd>CXXFLAGS="-fPIC" ./autogen.sh</kbd> and then hit enter</li>
                                    <li>type <kbd>./configure --prefix=/mingw64</kbd> and then hit enter</li>
                                </ol>
                            </li>
                            <li>
                                A few source code amendments:<br/>
                                <ol>
                                    <li>Open <code>movit-1.7.2/Makefile.in</code>, and then replace the whole <u>line number 24</u> with <code>CXXFLAGS=-Wall @CXXFLAGS@ -fvisibility-inlines-hidden -I$(GTEST_DIR)/include @Eigen3_CFLAGS@ @epoxy_CFLAGS@ @FFTW3_CFLAGS@ @benchmark_CFLAGS@</code>.</li>
                                    <li>Open <code>movit-1.7.2/effect_chain.cpp</code>, and then replace <code>(long)</code> with <code>(long long)</code>. There should be 5 such occurences to replace throughout the file.</li>
                                    <li>Open <code>movit-1.7.2/fft_convolution_effect.cpp</code>, and then add the following code below the #include pre-processor directives (at around <u>line number 14</u>):<br/>
                                    <code>#ifdef __MINGW64__<br/>#define ffs __builtin_ffs<br/>#endif</code></li>
                                </ol>
                            </li>
                            <li>In the command window, type <kbd>make libmovit.la install</kbd> and then hit enter</li>
                            <li>
                                In the command window:
                                <ol>
                                    <li>type <kbd>pacman -S mingw-w64-x86_64-libebur128 mingw-w64-x86_64-qt6-base mingw-w64-x86_64-qt6-declarative mingw-w64-x86_64-qt6-svg mingw-w64-x86_64-qt6-5compat mingw-w64-x86_64-jack2 mingw-w64-x86_64-rtaudio mingw-w64-x86_64-rubberband mingw-w64-x86_64-sox mingw-w64-clang-x86_64-clang mingw-w64-clang-x86_64-clang-tools-extra</kbd> and then hit enter</li>
                                    <li>type <kbd>pacman -S --needed base-devel mingw-w64-x86_64-gcc mingw-w64-x86_64-swig</kbd> and then hit enter</li>
                                    <li>type <kbd>pacman -S --needed gettext gettext-devel</kbd> and then hit enter</li>
                                </ol>
                            </li>
                            <li>
                                In the command window:
                                <ol>
                                    <li>type <kbd>cp -f C:/msys64/tmp/mlt-7.34.1/build/src/modules/jackrack/mltjackrack_export.h C:/msys64/tmp/mlt-7.34.1/src/modules/jackrack/mltjackrack_export.h</kbd> and then hit enter</li>
                                    <li>type <kbd>cp -f C:/msys64/tmp/mlt-7.34.1/build/src/modules/jackrack/mltladspa_export.h C:/msys64/tmp/mlt-7.34.1/src/modules/jackrack/mltladspa_export.h</kbd> and then hit enter</li>
                                    <li>type <kbd>ln -s /mingw64/lib/libjack64.dll.a /mingw64/lib/libjack.dll.a</kbd> and then hit enter</li>
                                </ol>
                            </li>
                            <li>Copy the folder path for the folder <code>synfig-1.5.3</code></li>
                            <li>
                                In the command window:
                                <ol>
                                    <li>type <kbd>cd</kbd> following by a space, and then paste the copied path and then hit enter</li>
                                    <li>type <kbd>./1-setup-windows-msys2.sh</kbd> and then hit enter</li>
                                </ol>
                            </li>
                            <li>
                                There is a bug in the <code>intltool-update</code> program that fails the Synfig build.<br/>
                                I have reported this to Ubuntu, and the bug report can be found at <a href="https://bugs.launchpad.net/intltool/+bug/2136814">this link</a>.<br/>
                                Open <code>C:/msys64/usr/bin/intltool-update</code>, and on <u>line numbers 628 through 631</u>, replace this:<br/>
                                <code>$srcdir =~ s#^../##;<br/>$dummy =~ s#^$srcdir/../##;<br/>$dummy =~ s#^$srcdir/##;<br/>$dummy =~ s#_build/##;</code>
                                <br/>with this:<br/>
                                <code>$srcdir =~ s#^\Q../\E##;<br/>$dummy =~ s#^\Q$srcdir/../\E##;<br/>$dummy =~ s#^\Q$srcdir/\E##;<br/>$dummy =~ s#_build/##;</code>
                            </li>
                            <li>In the command window, type <kbd>./2-build-debug.sh all full</kbd> and hit enter (be patient as this process takes a while)</li>
                        </ul>
                    </p>
                    <p>
                        &nbsp;&nbsp;&nbsp;<b>All done!</b><br/><br/>
                        <img src="images/scribblings/synfig_build_result.png" class="img-thumbnail" alt="Synfig Build Result #1" loading="lazy">
                    </p>

<br>

----------

* [Installation instructions for Windows, Linux, MacOS packages](https://synfig.readthedocs.io/en/latest/installation.html)
* [Building and installing from source](https://synfig-docs-dev.readthedocs.io/en/latest/building/Building%20Synfig.html)

## Credits

### Code Contributors

This project exists thanks to all the people who contribute. [[Contribute](https://synfig-docs-dev.readthedocs.io/en/latest/community/contribution%20guidelines.html)].
<a href="https://github.com/synfig/synfig/graphs/contributors"><img src="https://opencollective.com/synfig/contributors.svg?width=890&button=false" /></a>

### Financial Contributors

Become a financial contributor and help us sustain our community.

#### Individuals

<a href="https://opencollective.com/synfig"><img src="https://opencollective.com/synfig/individuals.svg?width=890"></a>

#### Organizations

Support this project with your organization. Your logo will show up here with a link to your website.

<a href="https://opencollective.com/synfig/organization/0/website"><img src="https://opencollective.com/synfig/organization/0/avatar.svg"></a>
<a href="https://opencollective.com/synfig/organization/1/website"><img src="https://opencollective.com/synfig/organization/1/avatar.svg"></a>
<a href="https://opencollective.com/synfig/organization/2/website"><img src="https://opencollective.com/synfig/organization/2/avatar.svg"></a>
<a href="https://opencollective.com/synfig/organization/3/website"><img src="https://opencollective.com/synfig/organization/3/avatar.svg"></a>
<a href="https://opencollective.com/synfig/organization/4/website"><img src="https://opencollective.com/synfig/organization/4/avatar.svg"></a>
<a href="https://opencollective.com/synfig/organization/5/website"><img src="https://opencollective.com/synfig/organization/5/avatar.svg"></a>
<a href="https://opencollective.com/synfig/organization/6/website"><img src="https://opencollective.com/synfig/organization/6/avatar.svg"></a>
<a href="https://opencollective.com/synfig/organization/7/website"><img src="https://opencollective.com/synfig/organization/7/avatar.svg"></a>
<a href="https://opencollective.com/synfig/organization/8/website"><img src="https://opencollective.com/synfig/organization/8/avatar.svg"></a>
<a href="https://opencollective.com/synfig/organization/9/website"><img src="https://opencollective.com/synfig/organization/9/avatar.svg"></a>

## SAST Tools

[PVS-Studio](https://pvs-studio.com/pvs-studio/?utm_source=website&utm_medium=github&utm_campaign=open_source) - static analyzer for C, C++, C#, and Java code.

## License

This project is licensed under the [GNU General Public License v3.0](https://github.com/synfig/synfig/blob/master/LICENSE).
