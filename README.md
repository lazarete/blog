# Blog Netlify + Hugo
    hugo new site nome_do_blog
    cd nome_do_blog
    execute o git init

# Instalando um tema
    URL do tema
    https://themes.gohugo.io/hugo-theme-hello-friend-ng/
    git submodule add https://github.com/rhazdon/hugo-theme-hello-friend-ng.git themes/hello-friend-ng
  
  
  # Configuração config.toml
    
    baseurl = "/"
    languageCode = "en-us"
    theme = "hello-friend-ng"
     
    [params]
    dateform        = "Jan 2, 2006"
    dateformShort   = "Jan 2"
    dateformNum     = "2006-01-02"
    dateformNumTime = "2006-01-02 15:04 -0700"
    
    # Set disableReadOtherPosts to true in order to hide the links to other posts.
    disableReadOtherPosts = false
    
    # Metadata mostly used in document's head
    description = "Homepage and blog by Djordje Atlialp"
    keywords = "homepage, blog, science, informatics, development, programming"
    images = [""]
    
    
    # Directory name of your blog content (default is `content/posts`)
    contentTypeName = "posts"
    
    # Default theme "light" or "dark"
    defaultTheme = "dark"
    
    [languages]
    [languages.en]
    title = "Hello Friend NG"
    subtitle = "A simple theme for Hugo"
    keywords = ""
    copyright = ""
    readOtherPosts = "Read other posts"
    
    [languages.en.params.logo]
      logoText = "hello friend ng"
      logoHomeLink = "/"
    # or
    #
    # path = "/img/your-example-logo.svg"
    # alt = "Your example logo alt text"
    
    # And you can even create generic menu
    [[menu.main]]
    identifier = "blog"
    name       = "Blog"
    url        = "/posts"
      

# Configuração netlify.toml
    [build]
    publish = "public"
    command = "hugo --gc --minify"

    [context.production.environment]
    HUGO_VERSION = "0.63.1"
    HUGO_ENV = "production"
    HUGO_ENABLEGITINFO = "true"
    
    [context.split1]
    command = "hugo --gc --minify --enableGitInfo"
    
    [context.split1.environment]
    HUGO_VERSION = "0.63.1"
    HUGO_ENV = "production"

    [context.deploy-preview]
    command = "hugo --gc --minify --buildFuture -b $DEPLOY_PRIME_URL"

    [context.deploy-preview.environment]
    HUGO_VERSION = "0.63.1"

    [context.branch-deploy]
    command = "hugo --gc --minify -b $DEPLOY_PRIME_URL"

    [context.branch-deploy.environment]
    HUGO_VERSION = "0.63.1"

    [context.next.environment]
    HUGO_ENABLEGITINFO = "true"
    

# Tetando as configurações local:
    hugo server -t hello-friend-ng
    
    Environment: "development"
    Serving pages from memory
    Running in Fast Render Mode. For full rebuilds on change: hugo server --disableFastRender
    Web Server is available at http://localhost:1313/ (bind address 127.0.0.1)
    Press Ctrl+C to stop


# .gitignore
    resources
    public
    
    
# Criando um post
    hugo new posts/titulo-do-post.md
    
    
# Adicionando perfil administrativo
    git submodule add https://github.com/woliveiras/netlify-cms-base.git static/admin
    
    
