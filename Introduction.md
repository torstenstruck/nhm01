# nhm01
nhm01 is the University of Oslo Natural History Museum's High Processing Computer. 

# Applying for an account on nhm01
If you need an account at nhm01, and you work at the museum, then email me at t.h.struck(@)nhm.uio.no. This email should include the short form of your UiO username. I’ll forward it on to the Drift team, and normally this gets sorted in about a week.

# Getting Started
Logging into nhm01 can be a little more challenging than some other similar HPCs, as it requires a "Proxy Jump". Proxy Jumps are a way to use another server (in this case the University's login server) as a middle-way between two points.
To log in to nhm01, use the following command, replacing \<user\> with your username:

```
ssh -J <user>@login.uio.no <user>@nhm01.hpc.uio.no 
```

To allow administration of your account by NHM please use the following command:

```
setfacl -R -m u:torsths:rwx /home/<user>
```

If you need to copy files to nhm01, you can do it like this:

```
scp -o 'ProxyJump <user>@login.uio.no' <filename> <user>@nhm01.hpc.uio.no:/destination/of/copy
```

# Storage usage (IMPORTANT TO BE READ BY EVERYONE) and where to store and run data
nhm01 is a relatively small HPC in comparison to the machines that you might be used to working on. nhm01's storage is partitioned in a very particular way. 

There are three main folders:

*/home:*

This folders contains your private home folder, it should only be used for private data and not your research and project data. No analyses should run here. You should only use minimal storage here. This folder has only a size of 300 Gb across all of users. Accordingly, your home directory should not have more than 5 Gb. This might be problematic with conda installations. Therefore please read the introduction to [Conda installations](Conda.md).

*/scratch:*

This folder is for running analyses. It is not for storage. All data should be stored on /storage. The folder will be checked at regular intervals. **All data that are not part of a running analysis and are older than 3 weeks will be deleted without contacting the owner of the folder.**

*/storage:* 

This folder is designed for easy access to stored data. Additionally, it is the folder which includes the software installations as modules, databases needed for different analyses and should be used for the [Conda installations](Conda.md) as well. You can also run analyses here without problems. Accordingly, it is the largest folder on NHM01 with 64 Tb. Even though it is the largest one, it can get full quite quickly as well.

You therefore need to clean up your folders in storage and keep it nice and structured there as well. The folder will be checked at regular intervals.

**All top-level folders (e.g., /storage/folder1/folder2/ or /storage/folder1/) that comprise only files older than 6 months are regarded as long-term storage and not in active usage. The owner of such folders will be contacted by the NHM01 manager to move the data to different storage solutions if not enough storage capacity is left on NHM01 (i.e., >10% of available space). As a very last resort if no action is taken by the owner, the NHM01 manager will compress and move the data to a local storage solution. At the local storage solution, it will be kept for another year. Before the final deletion of the data, the owner of the data will be contacted to find solutions for another storage solution.**

The goal is to have generally at least 10% of available storage capacity here. This should guarantee enough free space for compression of larger folders as well as analyses generating larger sized, temporary files. 

Cleaning up means: 
- Delete all files and folders you already know you will not need any longer (for example, where the run or installation failed). This just clutters the server with dead data and software.
- Move data you are not actively using to long-term storage solutions. The storage is not for long term storage or backup. [For more long-term storage please use other solutions such as offered by UiO](https://www.uio.no/english/services/it/research/storage/).
- Compress folders you will not use for some time but are still part of the project to save space.

```
tar -czvf dir-compressed.tar.gz /path/to/folder #(e.g., /home/torsths/test)
```

*Checking folder sizes:* 

You can check the size of a specific folder (or file) using the command:

```
du -h -d 1 /path/to/folder #(e.g., /home/torsths)
```

This will provide you with size of each folder at the first level (-d 1) in that path in a human readable format (-h). If you also want to get the information within the folders of the first level use, for example, -d 2 and so forth to get deeper into the folder structure.

You can find information about the general structure and how much storage is being used right now by using the command:

```
df -h
```

This will produce an output that looks like this

```
Filesystem                  Size  Used Avail Use% Mounted on
devtmpfs                   1006G     0 1006G   0% /dev
tmpfs                      1006G  4.3M 1006G   1% /dev/shm
tmpfs                      1006G  2.6M 1006G   1% /run
tmpfs                      1006G     0 1006G   0% /sys/fs/cgroup
/dev/mapper/internvg-root    10G  164M  9.9G   2% /
/dev/mapper/internvg-usr     10G  5.7G  4.4G  57% /usr
/dev/mapper/nvmevg-scratch  7.0T  4.2T  2.9T  60% /scratch
/dev/sda2                   508M  297M  211M  59% /boot
/dev/mapper/internvg-var     10G  7.4G  2.7G  74% /var
/dev/sda1                   200M  5.8M  195M   3% /boot/efi
/dev/mapper/internvg-opt     10G  998M  9.1G  10% /opt
/dev/mapper/internvg-tmp     10G  140M  9.9G   2% /tmp
/dev/mapper/datavg-storage   64T   27T   38T  42% /storage
/dev/mapper/internvg-home   300G  239G   61G  80% /home
tmpfs                       202G     0  202G   0% /run/user/326307
tmpfs                       202G     0  202G   0% /run/user/356492
tmpfs                       202G     0  202G   0% /run/user/342885
tmpfs                       202G     0  202G   0% /run/user/309450
tmpfs                       202G     0  202G   0% /run/user/322113
tmpfs                       202G     0  202G   0% /run/user/338283
```

As you can see here, all users on nhm01 share the filesystem /dev/mapper/internvg-home, better known as /home, which is currently 80% full in this example. Because of this limited storage in home (300GB shared between all users), we'd like users to be a bit more vigilant about where they store their data. We don't have individual user quotas, so that requires everyone to take a little more care.

# What these Technical Specifications mean practically
  For jobs that include fewer, large calculations, nhm01 is great, and might even be preferred vs., say, waiting in the bigmem queue on Saga. Genome Assembly, especially, is somewhere that nhm01 should excel.
  For jobs that include lots of little calculations, such as phylogenetic tree searches, nhm01 might not be the optimal choice.

# Generating folders
You can generate folders at the server using the mkdir command but to allow administration of it by NHM please use the following command:

```
mkdir <foldername>
setfacl -R -m u:torsths:rwx <foldername>
```

