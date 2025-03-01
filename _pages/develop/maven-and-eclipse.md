---
mediawiki: Using_Maven_with_Eclipse
title: Using Maven with Eclipse
---

To take full advantage of a Maven-based project from within Eclipse, you should install the Maven plugin according to these steps. Please note that you must be using Eclipse 3.5 or newer.

For more information about Maven, see:

-   [Maven web site](http://maven.apache.org/)
-   [Maven: The Complete Reference](http://www.sonatype.com/books/mvnref-book/reference/public-book.html)
-   [Better Builds with Maven](http://www.maestrodev.com/better-builds-with-maven/0-about-this-guide)

## Install m2eclipse

{% capture newer-eclipses-ship-m2e -%}
If you use Eclipse 3.7 Indigo or newer, you most likely have m2e already. If your {% include bc path='File | Import' %} offers you a *Maven* section, you have it, and can skip the rest of this section.
{%- endcapture -%}
{% include notice icon="note" content=newer-eclipses-ship-m2e %}

-   From the Eclipse menu, choose {% include bc path='Help | Install New Software...'%}
-   Click the Add button to add an update site:
    -   Name: m2eclipse
    -   Location: http://m2eclipse.sonatype.org/sites/m2e
    -   Click OK
-   Check the item that appears—Maven Integration for Eclipse—and click Next.
-   Click Next again, accept the license terms, and click Finish.
-   Restart Eclipse.

## Configure Eclipse (Windows only)

On Windows, you will need to configure Eclipse to use the Java Development Kit (JDK).

First, download and install JDK 6 from the Java web site.

Take note of where the Java installation ended up; we will use "C:\\Program Files\\Java\\jdk1.6.0\_20" below, though your path is likely to be different.

Also take note of the path to your Eclipse installation; we will use "C:\\Users\\you\\Programs\\eclipse" though your path is likely to be different.

Open the file "C:\\Users\\you\\Programs\\eclipse\\eclipse.ini" in a text editor, and follow [these instructions](http://wiki.eclipse.org/Eclipse.ini#specifying-the-jvm) to specify the JVM using the "-vm" flag. Be careful to follow the instructions exactly.

Now update Eclipse's JRE to be JDK-aware:

-   Launch Eclipse
-   From the menu choose {% include bc path='Window | Preferences'%}
-   Select {% include bc path='Java | Installed JREs'%}
-   Click Search..., navigate to "C:\\Program Files\\Java\\jdk1.6.0\_20" and click OK
-   Check the box next to the JDK that appears and click OK

## Troubleshooting

### On macOS, behind a firewall

I ([Jean-Yves](/people/tinevez) speaking) noticed a very annoying problem. At work, I live behind a firewall, and whatever I did, the version of maven shipped with Eclipse (Indigo or Juno) never managed to reach out the outside world (even tweaking proxy configuration). This resulted in the impossibility to import any maven project.

One solution that worked for me was to configure Eclipse not to use the maven version it shipped. Instead, I used the one shipped with [fink](http://pdb.finkproject.org/pdb/package.php/maven) accessible from the command line, and with a system proxy configured correctly.
