
The Nautilus application development team has been working on a project repository /opt/official.git. This repo is cloned at /usr/src/kodekloudrepos on storage server in Stratos DC. They recently shared the following requirements with DevOps team:


a. Create a new branch nautilus in /usr/src/kodekloudrepos/official repo from master and copy the /tmp/index.html file (on storage server itself). Add/commit this file in the new branch and merge back that branch to the master branch. Finally, push the changes to origin for both of the branches.


Solution
========    
- ssh into storage server

```
ssh natasha@ststor01
```
- Login as root
```
sudo su -
```
- Go to repository
```
cd /usr/src/kodekloudrepos/official
```
- Check the status of the repository
```
git status
```
- Create new branch
```
git checkout -b nautilus
```
- Copy the /tmp/index.html file (on storage server itself)
```
cp /tmp/index.html /usr/src/kodekloudrepos/official
```
```
ll  /usr/src/kodekloudrepos/official
```
- Add the file
```
git add index.html
```
- Commit the changes
```
git commit -m "index.html added"
```
```
git branch
```
- Change to the master branch
```
git checkout master
```
- Merge the branch
```
git merge nautilus
```
```
git push -u origin nautilus
```
```
git push -u origin master
```
