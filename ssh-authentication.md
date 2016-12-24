# Switch to SSH Authentication

GitHub requires authentication for _git push_ and other 'write' operations.
GitHub supports

  * https
  * ssh

authentication.

The GitHub documentation prefers _https_ but does not really say why.
Reading between the lines, this option requires no further set up and so works the same way regardless of the user's operating system platform.

GitHub describes (somewhat misleadingly) setting up _ssh_ authentication as a four stage process.
The descriptions are clearly for Linux, where the use of _ssh_ is mundane.

Our 'set-up' involved an extra security minded bell and whistle.

---

The four stages of the GitHub process are identified in the [Generating an SSH key] (https://help.github.com/articles/generating-an-ssh-key/) help article:

  * [Checking for existing SSH keys] (https://help.github.com/articles/checking-for-existing-ssh-keys/)
  * [Generating a new SSH key and adding it to the ssh-agent] (https://help.github.com/articles/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent/)
  * [Adding a new SSH key to your GitHub account] (https://help.github.com/articles/adding-a-new-ssh-key-to-your-github-account/)
  * [Testing your SSH connection] (https://help.github.com/articles/testing-your-ssh-connection/)

These help articles have further links of which the following were read:

  * [Working with SSH key passphrases]  (https://help.github.com/articles/working-with-ssh-key-passphrases/)
  * [Changing a remote's URL]  (https://help.github.com/articles/changing-a-remote-s-url/)
  * [Error: Permission denied (publickey)]  (https://help.github.com/articles/error-permission-denied-publickey/)

---

## Checking for Existing SSH Keys

This is straight forward if you know how.

Of course you can reuse an SSH key you already have just as you can reuse a password you use in other contexts to log in to GitHub:
it is not a good idea.

I don't ever use a domestic password at work or on the Internet.
On the Internet I use different passwords for different sites so that if any become compromised, they are not all compromised.


## Generating a new SSH key and adding it to the ssh-agent

This is familiar enough:

    $ cd ~/.ssh
    $ ssh-keygen -t ecdsa -C "NewForester@users.noreply.github.com" -f github

This is an elliptical DSA password.
Supposedly even stronger and recommended now that processors are brute enough to crack DSA.
I noted that the deprecated RSA is still used in many of the GitHub examples.

The GitHub help expects you to use a passphrase.
I've never done this.
I've no real objection.
It is just I have a background in automation where any kind of manual intervention is not really on.

### Working with SSH key passphrases

Not using a passphrase, this help article suggested, is no better than using a random password and writing it down.

It proposed using _ssh-agent_ to avoid having to re-enter the passphrase.
This does not sound like it achieves anything.

However, with _ssh-agent_ you still have to enter the passphrase once.
It is then cached so that you do not need to re-enter it.

How long is the passphrase cached ?  I am used to seconds or minutes, not hours or days.
Since the help article said nothing I took it to mean until the _ssh-agent_ is shut down.
I guess it is configurable.

The other potential advantage of using ssh-agent is that it deals with ssh keys on behalf of the application so you do not need to trust every application, only _ssh-agent_.

Hmm ... _ssh-agent_ is already started when I log in so perhaps it is already caching credentials.
No further action needed.


## Adding a new SSH key to your GitHub account

This involves using the GitHub UI.
Instructions for UI use require JavaScript.
If JavaScript is disabled (for example because you use NoScript), the instructions will simply not be displayed
and you're left wondering why the page contents do not describe what the page title suggests they should.

Adding the key tp GitHub was straight forward enough:

   * navigate to your GitHub profile
   * select "SSH and GPG keys"
   * click on "New SSH key"
   * enter a title
   * copy/paste in your SSH key (the public half)

I did a simple _cat_ and copied/pasted the one line of output.
The help article suggest using _xclip_ but I didn't have it installed.

You will get a confirmation e-mail from GitHub,
just so that you know when someone hacks your account and adds such a key.


## Testing your SSH connection

This is simple:

    $ ssh -T git@github.com

and confirm that you want to connect to _github.com_.
This will, with default settings, add the site to your list of known hosts
so you will not be asked again.

If it works, you end with the message:

    Hi NewForester! You've successfully authenticated, but GitHub does not provide shell access.

It didn't work.  I got:

    Error: Permission denied (publickey)


### Error: Permission denied (publickey)

There is a help article for troubleshooting this error message.

The article did not cover my problem but it was nice to be able to eliminate the more obvious causes without pain.

With:

    $ ssh -vT git@github.com

I was able to see how _ssh_ was trying to connect and so why it was not working.

It came down to my use of _-f github_ when I generated my key:
by default, _ssh_ was looking for key files with default names
and was not even bothering to try the key in _~/.ssh/github_.


### Changing a remote's URL

Why this is a almost an afterthought to "Testing your SSH connection" and not the fifth step I don't know.

It is easy enough but it needs doing for each repository:

    $ git remote -v
    github  https://github.com/NewForester/test (fetch)
    github  https://github.com/NewForester/test (push)

    $ git remote set-url github git@github.com:NewForester/test

    $ git remote -v
    github  git@github.com:NewForester/test (fetch)
    github  git@github.com:NewForester/test (push)

---

## The Bell and Whistle

I had generated a key in a file with a name other than the default and it was ignored.
I needed to fix that.

I also want the key to be used only for GitHub access and I want only this key to be used.
No need to waste time trying keys that will not work even if that does not involve any risk.

Both desiderata can be met conveniently with a local (user specific) _ssh_ configuration:

    $ cd ~/.ssh
    $ cat >> config
    Host = github.com
        IdentityFile = ~/.ssh/id_github
    $ chmod 600 config
