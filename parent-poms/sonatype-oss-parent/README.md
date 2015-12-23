Cyc Sonatype OSS Parent
=======================

[![Maven Central](https://img.shields.io/maven-central/v/com.cyc/cyc-sonatype-oss-parent.svg)]()

Provides Sonatype deployment configuration for Cycorp's open source software.


Usage
-----

Full documentation on how to release artifacts to Sonatype OSSRH is beyond the scope of this README,
but basic usage of this POM (via projects which inherit from it) is as follows...

From the root of the project to release, run the following:

    mvn clean deploy -Dsonatype-oss-release

You will be prompted for your GPG passphrase. 

_Warnings:_

* Projects with versions ending in "-SNAPSHOT" will be silently ignored by OSSRH.
* Depending on how your environment is configured, Maven may ask you for your GPG passphrase 
  countless times. If this happens, you'll want to install gpg-agent. See _Using gpg-agent_, below.
* A GUI popup will be created by gpg-agent for your key (if you're using gpg-agent).
* Don't run this in GNU Screen. If you do, the aforementioned popup will be swallowed.

If your staging is successful, it will conclude by spitting out a staging repository ID:

    ...
    Uploaded: https://oss.sonatype.org:443/service/local/staging/deployByRepositoryId/... (2546 KB at 3765.0 KB/sec)
    [INFO]  * Upload of locally staged artifacts finished.
    [INFO]  * Closing staging repository with ID "[STAGING_REPOSITORY_ID]".

    Waiting for operation to complete...
    ......

    [INFO] Remote staged 1 repositories, finished with success.
    [INFO] ------------------------------------------------------------------------
    [INFO] Reactor Summary:
    [INFO]
    [INFO] ...
    [INFO] ------------------------------------------------------------------------
    [INFO] BUILD SUCCESS
    [INFO] ------------------------------------------------------------------------
    [INFO] Total time: 02:46 min
    [INFO] Finished at: [DATE/TIME]
    [INFO] Final Memory: 46M/492M
    [INFO] ------------------------------------------------------------------------

Look for the line `[INFO]  * Closing staging repository with ID "[STAGING_REPOSITORY_ID]".` That has 
the staging repository ID.

_Note:_ If you do not see the line _Closing staging repository with ID "foo"_, the upload has 
failed. Unhelpfully, Maven will still report this as a success.


### Viewing your staging repositories

There are a couple of ways to view your staging repositories. The most direct way:

1. Go to <https://oss.sonatype.org/#stagingRepositories>. Log in if with your Sonatype JIRA ID if
   you are not already logged in.
2. In the top-left pane (the _Staging Repositories_ tab) scroll down to your repository (this should
   be named after "[STAGING_REPOSITORY_ID]").
3. Click on it. Details will appear in the lower pane.

Alternately, you can see all of your artifacts at 
<https://oss.sonatype.org/#nexus-search;quick~[GROUP_ID]>. (E.g., 
<https://oss.sonatype.org/#nexus-search;quick~com.cyc>)

### Releasing the Repository via the Web

1. Go to <https://oss.sonatype.org/#stagingRepositories>. Log in if with your Sonatype JIRA ID if
   you are not already logged in.
2. In the top-left pane (the _Staging Repositories_ tab) scroll down to your repository 
   ("[STAGING_REPOSITORY_ID]"). Click on it.
3. Details  will appear in the lower pane. More importantly, some buttons at the very top of the
   _Staging Repositories_ tab will become enabled: **Release** and **Drop**.
4. If you're unhappy with the repository, click "Drop".
5. If you like the staged repository, click "Release" to publish it to Maven Central.

After a minute or two, your artifacts should be visible in Sonatype's OSSRH release repository:
<https://oss.sonatype.org/content/repositories/releases/[GROUP_ID_AS_PATH]>. (E.g.,
<https://oss.sonatype.org/content/repositories/releases/com/cyc/>)

You can also see all of your artifacts at <https://oss.sonatype.org/#nexus-search;quick~[GROUP_ID]>
(E.g., <https://oss.sonatype.org/#nexus-search;quick~com.cyc>)

After a while (anywhere from a couple of hours to a couple of days), the new artifacts should be
viewable here:

* <http://search.maven.org/#search%7Cga%7C1%7C[GROUP_ID]> 
  (e.g., <http://search.maven.org/#search%7Cga%7C1%7Ccom.cyc>)
* <http://mvnrepository.com/artifact/[GROUP_ID]> 
  (e.g., <http://mvnrepository.com/artifact/com.cyc>)

For more info, see <http://central.sonatype.org/pages/releasing-the-deployment.html>.


### Using gpg-agent

You don't have to use gpg-agent, but you will probably really, really want to.

If your machine does not have gpg-agent installed, Maven may ask you for your GPG passphrase
countless times:

    ...
    [INFO] --- maven-gpg-plugin:1.5:sign (sign-artifacts) @ cyc-base-client ---
    
    You need a passphrase to unlock the secret key for
    user: "USERNAME <EMAIL@DOMAIN>"
    2048-bit RSA key, ID [ID], created [DATE]
    
    Enter passphrase: gpg: gpg-agent is not available in this session
    
    You need a passphrase to unlock the secret key for
    user: "USERNAME <EMAIL@DOMAIN>"
    2048-bit RSA key, ID [ID], created [DATE]
    
    Enter passphrase: gpg: gpg-agent is not available in this session
    
    ...

... And, if you enter your GPG passphrase countless times, everything will eventually go just fine.
Well, your patience will slowly be ground down to a light, fluffy powder, but everything will go 
just fine for _Maven._

On Linux, to test whether gpg-agent is installed on your system:

    which gpg-agent

Install gpg-agent if necessary. Then, add the following to your ~/.bashrc.local, or manually 
evaluate it before performing a release:

    eval $(gpg-agent --daemon --no-grab --write-env-file $HOME/.gpg-agent-info)
    export GPG_TTY=$(tty)
    export GPG_AGENT_INFO

**Note:** There is some advice online to store your GPG passphrase in your settings.xml or wherever.
This is terrible advice.

See: [Avoid gpg signing prompt when using Maven release plugin](http://stackoverflow.com/a/25197868/786623)


Further Documentation
---------------------

For more information about Cyc open source software, visit the [Cyc Developer Center](http://dev.cyc.com/).


Contact
-------

Issues may be reported via [Cyc issue tracker](http://dev.cyc.com/issues/).

