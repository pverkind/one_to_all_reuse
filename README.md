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