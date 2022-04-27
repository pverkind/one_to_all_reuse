# one_to_all_reuse
reuse data for one text vs all texts in the corpus


Git tag experiment: 

1. Created a repo, https://github.com/pverkind/one_to_all_reuse.git, with a README.md file
2. Added 2 dummy files to it, with content "DUMMY DATA - V1":
* 2021.2.5_Shamela0010283_all.csv
* 2021.2.5_Shamela0011680_all.csv
3. Add + commit
4. Add a tag number and message: 
  ```
  git tag -a v1.0 -m "First version with dummy data"
  ```
5. List all tags in the repo: `git tag`
6. Push the repo to GitHub: `git push origin main`
7. Result: the tag does not appear on GitHub! 
  By default, `git push` does not transfer tags to remote servers! 
  You have to push tags explicitly:
  ```
  git push origin v1.0
  ```
8. Check whether you can access it using raw.githubusercontent (it works, but only after a couple of minutes):
https://raw.githubusercontent.com/pverkind/one_to_all_reuse/v1.0/2021.2.5_Shamela0010283_all.csv
9. Update the two dummy files with content “DUMMY DATA – V2” and change their names to lose the version inside the file name: 
* Shamela0010283_all.csv
* Shamela0011680_all.csv
10. Add and commit the changes
11. Add a tag to this specific commit: 
a. Get the checksum of the commit: `git log --pretty=oneline`
  ```
  ca34244c33ff0f91c07b6a5a4a249e7433cf095f (HEAD -> main) Rename files and change dummy content
  1d743f61230939718f5b233b132b498418bd2d9a (tag: v1.0, origin/main, origin/HEAD) Add dummy files for V1
  5cafc96ca5a26076860e950a8702099ffd4e08d0 Initial commit
  ```
b. Tag the commit by including part of the checksum in the git tag command:
   ```
   git tag -a v2.0 ca34244 "Second version, still with dummy data"
   ```
12. Push the tag: `git push origin v2.0`
13. Something interesting has happened:
  * the tag is visible on GitHub, with a reference to the commit, and a zip file that contains the entire repo at the state of this commit/tag (the files in this zip file contain the text "DUMMY DATA - V2")
  * BUT: the commit itself has not been pushed; the repo is still at its v1.0 state (the files in the repo still contain the text "DUMMY DATA - V1")
14. So we'll need to push the data itself separately too: `git push origin main`
15. Since the tags contain all data as a zip file, it's probably better to use lightweight tags, which do not include the data: 
a. Update the README.md file, add and commit it
b. Add a lightweight tag (omit the "-a" flag, and the message): `git tag v2.1`
c. Push the tag to GitHub: `git push origin v2.1`
d. Surprise: the same thing happened as with the normal tags: a zip file is created that contains the data at the time of the latest commit, even if this latest commit has not yet been pushed to GitHub. 
16. git push origin main

I have asked a question about this on StackOverflow: https://stackoverflow.com/questions/72025963/github-git-tag-without-creating-zip-file

17. check the size of the repo using the GitHub API: https://api.github.com/repos/pverkind/one_to_all_reuse
    the current size is listed as 2
18. Replace the files with files containing the real content. 
19. Add and commit the files
20. git tag v3.0
21. git push v3.0
22. Check the repo size again using the GitHub API: it didn't change! So it doesn't take the zip file into account?
23. git push origin main
24. Check the repo size again: still didn't change!!!

25. Check the local repo size: Size: 7.20MB, Size on disk: 7.25 MB; 54 Files, 32 Folders 
26. Add a file to the folder : 0346Istakhri.MasalikWaMamalik.Shamela0011680-ara1.mARkdown (586 / 588 KB)
27. Add and commit
28. Check the local repo size: Size: 7.98MB, Size on disk: 8.02MB, 59 files, 35 folders
29. `git tag v4.0`
30. Check the local repo size: Size: 7.98MB, Size on disk: 8.02MB, 60 Files, 35 Folders

## git branch experiment: 

* local repo size before: Size: 8,366,530 bytes; Size on disk: 8,413,184 bytes; 60 Files; 35 Folders
* Add another file: 0346Istakhri.MasalikWaMamalik.Shamela0010791-ara2 (571,834 / 573,440 bytes)
* local repo size befor adding and committing: 
  Size: 8,938,364 bytes; Size on disk: 8,986,624 bytes; 60 Files; 35 Folders 
  (NB: I saved the README file before, so the working version of it disappeared; that’s probably why it still has 60 files)
* git add 0346*
* local repo size after adding, before committing: 
  Size: 9,134,748 bytes; size on disk: 9,183,232 bytes; 61 files; 36 folders
* git commit –m “Add another text”
* local repo size after adding and committing: 
  Size: 9,135,529 bytes; size on disk: 9,183,232 bytes; 63 files; 37 folders
  (NB: size changed, but size on disk remains identical; 1 folder, 2 files added)
* Create a new branch called test: `git branch test`
* local repo size after branching:
  Size: 9,135,734 bytes; size on disk: 9,183,232 bytes; 65 files; 37 folders
    (NB: size increased, but size on disk remains identical; 0 folders, 2 files added)
* checkout the test branch: git checkout test 
* local repo size after checking out test branch:
  Size: 9,135,907 bytes; size on disk: 9,183,232 bytes; 65 files; 37 folders
    (NB: size increased very slightly, but size on disk remains identical; 0 folders, 0 files added)
* go back to main branch: git checkout main
* local repo size after checking out main branch:
  Size: 9,136,080 bytes; size on disk: 9,183,232 bytes; 65 files; 37 folders
    (NB: size increased very slightly, but size on disk remains identical; 0 folders, 0 files added)


| when | Size      | Size on disk | Files | Folders | Notes |
|------|-----------|--------------|---------|-------|-------|
| Before adding text file | 8,366,530 b |  8,413,184 b | 60 | 35 |  |
| After inserting text file | 8,938,364 b |  8,986,624 b | 60 | 35 |  |
| After add, before commit | 9,134,748 b |  9,183,232 b | 61 | 36 |  |
| After commit | 9,135,529 b |  9,183,232 b | 63 | 37 | size changed, but size on disk remains identical; 1 folder, 2 files added |
| After branch | 9,135,734 b |  9,183,232 b | 65 | 37 | size increased, but size on disk remains identical; 0 folders, 2 files added |
| After checking out new branch | 9,135,907 b |  9,183,232 b | 65 | 37 | size increased very slightly, but size on disk remains identical; 0 folders, 0 files added |
| After checking out main branch | 9,136,080 b |  9,183,232 b | 65 | 37 | size increased very slightly, but size on disk remains identical; 0 folders, 0 files added |
