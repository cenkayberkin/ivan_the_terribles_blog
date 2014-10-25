#Fixed problems with the blog.

- XSS problem: user input was embedded in the page without escaping so it was potential problem, user might inject many things to url and would appear on our page. So this is how i fixed it

```sh
<%= params[:status] == "published"? "published" : "unpublished" %> 
```
- SQL injection:
This line  
```sh
where("title like '%#{search}%'")
```
changed to 
```sh
where("title like ?", "%#{search}%")
```
So we shouldnt inject user input into db query directlly. User can do many things to our db.

- Updated to latest version of 3.2
- respnse header prevention method changed to 1 
```sh
response.headers['X-XSS-Protection'] = '1'
```

# My New Blog!

I've got a lot to say, and now I have a place to say it!!!!!

Read all my amazing posts!!!!! You can load them into the app with: `rake load:blog`

Since I know you want to read them all, I designed my page to show EVERYTHING on the front page of the site!!!!!

I know it is a little slow (but totes worth it!!!!)... _Do you know how I can make it faster?_

# I'm feeling insecure...

Well, I got some requirements from marketing to make sure we distinguish between published and unpublished posts. I made some changes to the html and css.

But now QA is telling me I have some security vulnerabilities! They were able to use the strings below to hack my site!!!

What do I do to fix it???


XSS:
```
http://localhost:3000/posts?utf8=%E2%9C%93&search=archive&status=foo=%22bar%22%3E%3Cscript%3Ealert%28%22p0wned!!!%22%29%3C/script%3E%3Cp%20data-foo
```

SQL Injection:

```
foo%'); INSERT INTO posts (id,title,body,created_at,updated_at) VALUES (99,'hacked','hacked alright','2013-07-18','2013-07-18'); SELECT "posts".* FROM "posts" WHERE (title like 'hacked%
```
