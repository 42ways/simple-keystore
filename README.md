# simple-keystore

This is a very simple keystore, implemented for a small team in the early 2000s to track and share stuff like infrastructure information, credentials etc. using PGP encryption.

To keep it simple and convinient, scripts and data are stored together in the same repository. The update of your workspace (retrieval of the latest data), the commit of changed data and the push to the repository are all part of the scripts (using git).


## Usage

### show-keys

Update the workspace, decrypt the file `keys.asc` to `keys` and show it's contents on the console (using `less`).

The file `keys.asc` has to be encypted using your public pgp-key in order to be able to decrypt it.

The temporary plain text files will be overridden with random data.

### edit-keys

Update the workspace (`git pull`), decrypt the file `keys.asc` to `keys` and open an editor (using the VISUAL environment variable, defaults to "vi" if not set). After leaving the editor, changes will be commited and pushed back to your repository.

The initial file `keys.asc` has to be encypted using your public pgp-key in order to be able to decrypt it. It will be encrypted with all keys listed in `encypt-to-users` after editing.

You will be prompted for a commit comment.

The temporary plain text files will be overridden with random data.

## Files

### encrypt-to-users

List of PGP addresses to encrypt the keys file. The public keys of all addresses must be available for your GPG installation.

### sign-user

(Optional) PGP address for signing the keys file. Will be your GPG user if omitted.

## Requirements

* GPG
* git
* perl (for overwriting the plain text files with garbage. Feel free to implement `erase-keys` in a language of you choise if the annoys you)

## Bootstrap

In order to install, you have to setup a git repository you want to use for your secret information and add the files from this project by following these steps:

* create an empty git repository on a trustworthy git server (a local machine is highly recommended for this!)
* clone the newly created repository to your workstation
* pull the content of this repository into your empty workspace
  * `git pull https://github.com/42ways/simple-keystore.git`
* edit the files `encrypt-to-users` and optionally `sign-user` to include the addresses you want to use
* create your initial `keys.asc` file, e.g. by using the provided plain text `keys.sample`, encrypting and adding it to your repo
  * `cp keys.sample keys`
  * `gpg --sign --encrypt --armor -r <YOUR_GPG_USER_ID> keys`
  * `git add keys.asc`
  * `git commit -avm 'add initial keys.asc'`
* do an initial edit of your new key store by calling `edit-keys` (you will be prompted if you want to reuse the still existing plain text file) and enter some information into the file

Now the file `keys.asc` should have been updated with your newly entered information (prove this with `show-keys`) and the changes should have been pushed to your remote.

