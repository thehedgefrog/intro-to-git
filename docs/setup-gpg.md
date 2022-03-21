# Set up GPG

## Generate a GPG key pair
To start, generate a new GPG key pair (public and private):

``` sh
gpg --full-gen-key
```

Configure the key with:

1. Kind of key: type `4` for `(4) RSA (sign only)`
2. Keysize: `4096`
3. Expiration: choose a reasonable value, for example `2y` for 2 years (it can be renewed)

Then answer a few questions:

1. Your real name. You could use your GitHub username here if you’d like.
2. Email address. If you plan to use this key for more than just Git, you might want to put your real email address. If it’s just for GitHub, you can use the `@users.noreply.github.com` email that GitHub generates for you: you can find it on the Email settings page.

You will be asked to type a passphrase which is used to encrypt your secret key on disk. This is important, otherwise attackers could steal your secret key, and then they’d be able to sign messages and Git commits pretending to be you.

You can verify your key was created with:
```
$ gpg --list-secret-keys --keyid-format SHORT
/root/.gnupg/pubring.kbx
------------------------
sec   rsa4096/674CB45A 2020-05-16 [SC] [expires: 2022-05-16]
      65B8A7455C949E73FC3B7330C16132F5674CB45A
uid         [ultimate] ItalyPaleAle-demo <43508+ItalyPaleAle@users.noreply.github.com>
```

In the example above, my new key ID is `rsa4096/674CB45A`, or just `674CB45A`.

You can confirm that GPG is working and able to sign messages with:
``` sh
echo "hello world" | gpg --clearsign
```

## Configure Git to sign your commits

Once you have your private key, you can configure Git to sign your commits with that:

*Replace 674CB45A with your key ID*
``` sh
git config --global user.signingkey 674CB45A
```

Now, you can sign Git commits and tags with:

* Add the `-S` flag when creating a commit: `git commit -S`
* Create a tag with `git tag -s` rather than `git tag -a`

You can also tell Git to automatically sign all your commits:
``` sh
git config --global commit.gpgSign true
git config --global tag.gpgSign true
```

## Adding the GPG key to GitHub

In order for GitHub to accept your GPG key and show your commits as “verified”, you first need to ensure that the email address you use when committing a code change is both included in the GPG key and verified on GitHub.

To set what email address Git uses when creating a commit use:
``` sh
git config --global user.email your@email.com
```

You can use your `@users.noreply.github.com` email (from the [Email settings](https://github.com/settings/emails) page on GitHub) or any other email address that is added to your GitHub account and verified (in the same settings page).

If it’s not already, that same email address must also be added to your GPG key, as per instructions above.

Once you’ve done it, upload your public GPG key to GitHub and associate it with your account. In the [SSH and GPG Keys settings](https://github.com/settings/keys) page, add a new GPG key and paste your public key, which you can get with:

*Replace 674CB45A with your key ID*
``` sh
gpg --armor --export 674CB45A
```

Your public GPG key begins with `-----BEGIN PGP PUBLIC KEY BLOCK-----` and ends with `-----END PGP PUBLIC KEY BLOCK-----`.

## Making a signed commit

After configuring all of the above, your Git commits can now be signed with your GPG key:

*Add the -S flag if you did not configure Git to sign commits by default*
``` sh
git commit -a -m "Making my first signed commit"
```

You can check that the commit was signed with:
```
$ git log --show-signature -1
commit 8beed807e820d34cc7a35a0d69e9913bed7b1b03 (HEAD -> master)
gpg: Signature made Sun May 17 01:44:55 2020 UTC
gpg:                using RSA key 674CB45A
gpg: Good signature from "ItalyPaleAle-demo <43508+ItalyPaleAle@users.noreply.github.com>" [ultimate]
Author: ItalyPaleAle-demo <43508+ItalyPaleAle@users.noreply.github.com>
Date:   Sun May 17 01:44:55 2020 +0000

    Making my first signed commit
```