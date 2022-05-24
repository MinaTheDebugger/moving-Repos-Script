#!/usr/bin/env bash

# Repositories
repos=(
Repo-name-OnE
rePO-name-2
)

# renaming repos, then just
# make sure reporenames=$repos
reporename=(
repo-one
repo-two
)

for ((x=0;x < ${#repos[@]};x++)); do                                                                                                              
  echo ${repos[$x]}                                                                                                                               
  echo ${reporenames[$x]}                                                                                                                         
  if [ -d "${repos[$x]}" ]; then                                                                                                                  
    echo "repo exists locally"                                                                                                                    
    continue;                                                                                                                                     
  fi      
  #
  #  change this to where the current repo is
  # for Example git@github.com:minathedebugger/REPONAME.git
  # You only need to edit the first part
  #                                                                                                                                        
  git clone git@github.com:JoshuaEstes/${repos[$x]}.git                                                                                              
  cd ${repos[$x]}                                                                                                                                 
  for branch in $(git branch -r | grep -v 'origin\/HEAD' | awk '{ print $1 }' | sed 's/origin\///g'); do                                          
    git checkout -B $branch origin/$branch                                                                                                        
    git pull origin $branch                                                                                                                       
  done;
  #
  # You will need to make sure this points to the new git repo
  #
  git remote set-url origin git@git.localhost:${reporenames[$x]}.git                                                                         
  git push -u origin                                                                                                                              
  cd ..                                                                                                                                           
done;
