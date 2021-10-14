gem install jekyll bundler



jekyll new resume



cd resume





bundle exec jekyll serve



에러 발생

```ruby
C:/Ruby30-x64/lib/ruby/gems/3.0.0/gems/jekyll-4.2.0/lib/jekyll/commands/serve/servlet.rb:3:in `require': cannot load such file -- webrick (LoadError)
```



bundle add webrick



http://localhost/4000/



![image](https://user-images.githubusercontent.com/58774664/133925357-7439a412-d654-4dfa-b1d1-fecfb69193da.png)

