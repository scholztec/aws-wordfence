# aws-wordfence


Using wordfence is awesome.
Using wordfence on AWS Lightsail preconfigured instances is a pain.
Here's how to fix 'user.ini' being accessible.

cp /opt/bitnami/apache/conf/vhosts/htaccess/wordpress-htaccess.conf /opt/bitnami/apache/conf/vhosts/htaccess/wordpress-htaccess.conf.old
sudo nano /opt/bitnami/apache/conf/vhosts/htaccess/wordpress-htaccess.conf

Paste the following at the bottom of the file, then save, exit, and reboot.
<Directory "/opt/bitnami/wordpress/">
<IfModule mod_rewrite.c>
        RewriteEngine On
        RewriteCond %{REQUEST_URI} ^/?\.user\.ini$
        RewriteRule .* - [F,L,NC]
</IfModule>
<IfModule !mod_rewrite.c>
        <Files ".user.ini">
        <IfModule mod_authz_core.c>
                Require all denied
        </IfModule>
        <IfModule !mod_authz_core.c>
                Order deny,allow
                Deny from all
        </IfModule>
        </Files>
</IfModule>
# Wordfence WAF
<Files ".user.ini">
<IfModule mod_authz_core.c>
        Require all denied
</IfModule>
<IfModule !mod_authz_core.c>
        Order deny,allow
        Deny from all
</IfModule>
</Files>
</Directory>
