This repo provides a quick template for combining projects from multiple GitHub repos into one
Visual Studio solution (e.g. to make changes across multiple projects for pull requests).

## Using this repo
The commands assume Bash, which you can run in a terminal in Linux/Mac or using
[Git Bash](https://gitforwindows.org/) on Windows.

### First-time setup
1. Clone this repo.
2. Edit the list of GitHub repos in `repo-list.txt`.
3. Run this from the solution folder to clone the repos:

   ```bash
   cat repo-list.txt | grep -e '^[^#]' | while read -r repo; do
      git clone https://github.com/$repo.git;
   done
   ```

4. Open the solution in Visual Studio.
5. Add the projects to the solution.

### Add fork remotes
If you forked the original repos on GitHub, this sets your fork as the default remote and sets the
original repo as `upstream`.

```bash
githubName=ExampleName # set this to your GitHub username

cat repo-list.txt | grep -e '^[^#]' | while read -r repo; do
   (
      repoName=$(echo $repo | sed -re 's/^[^\/]+\/(.+)$/\1/');
      cd $repoName;
      git remote rename origin upstream;
      git remote add origin https://github.com/$githubName/$repoName.git;
      git fetch --all --verbose;
   )
done
```

### Commit changes
Although you can make changes across all repos, each mod still has its own separate repo. To commit
changes, you'll need to open each individual repo folder and commit the changes there.
