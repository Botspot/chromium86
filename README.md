# chromium86
Downgrade to chromium-browser v86 on a Raspberry Pi

## To install
```
wget https://github.com/Botspot/chromium86/releases/download/86/chromium86.zip
unzip chromium86.zip
rm chromium86.zip
sudo cp -af ~/chromium86/. /
```

## To revert back to latest chromium-browser
```
sudo apt install --reinstall chromium-browser chromium-browser-l10n chromium-codecs-ffmpeg-extra rpi-chromium-mods
```

### How to create your own archives for any package(s) you want

1. Determine what files a package installs. I use `synaptic` package manager: Right-click on the package name -> Properties -> Installed files -> Ctrl+A -> Ctrl+C -> Ctrl+V into a text editor. Repeat the same steps for every package you want to archive. In this case, I repeated this process for `chromium-browser`, `chromium-browser-l10n`, `chromium-codecs-ffmpeg-extra`, and `rpi-chromium-mods`.
2. Save the text file to something like `/home/pi/Desktop/chromium_files.txt`.
3. Notice that the list of files also contains folders. We don't want those. To get rid of folders, run this command to operate on your text file:
```
echo "$(for i in $(cat /home/pi/Desktop/chromium_files.txt);do if [ -f "$i" ];then echo "$i";fi;done | sort | uniq)" > /home/pi/Desktop/chromium_files.txt
```
4. Create a directory to save your archived files in:
```
mkdir /home/pi/chromium86
```
5. Let's copy each file listed in our file-list, into that directory:
```
for i in $(cat /home/pi/Desktop/chromium_files.txt);do cp -a --parents "$i" /home/pi/chromium86;done
```
6. Almost done! At this point, you could zip up the entire folder's files, or you could upload the entire directory to Github as-is. One thing to beware of: Github doesn't allow any file above 100 megabytes. So in [my chromium78 repo](https://github.com/Botspot/chromium78), one file was larger than 100MB, so I compressed just that one. This time, two files are > 100MB. So to keep things simple, I'll just upload the entire folder as one big zip file, to the releases page (where filesize is limited to 2GB instead of 100MB)
