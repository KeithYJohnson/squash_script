# squash_script
bash squash branch script
```bash
  if [ $(git log HEAD..master | wc -l) -gt 0 ]
  then
    echo "ERROR: The branch to squash is missing commit(s) from master, merge and get tests passing first..."
  else
    # Get the current Branch.
    branch_to_squash="$(git symbolic-ref --short HEAD)"
    new_branch=$branch_to_squash"_squashed"
    checkout_msg="Checking out new branch '$new_branch' to squash on"
    echo $checkout_msg
    git checkout -b $new_branch
    git reset --soft master
    git commit
    echo "Squash Complete!"
    git checkout $branch_to_squash
    if [ $(git diff $new_branch | wc -l) -gt 0 ]
    then
      echo "ERROR: The squashed branch is not identical to original feature branch!"
    else
      echo "Success! The $branch_to_squash branch is identical to $new_branch"
      echo "To publish the squashed branch run: git push origin $new_branch:$branch_to_squash -f"
    fi
  fi
  ```
