sudo apt install git -y
git config --global user.name "seburt"
git config --global user.email "seburt1980@gmail.com"
git config --global core.editor nano

git config --local user.name "seburt"
git config --local user.email "seburt1980@gmail.com"

git remote add origin https://github.com/OWNER/REPOSITORY.git # Set a new remote
git remote -v # Verify new remote