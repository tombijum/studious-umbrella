hello ~
QN WHY DO I NEED VENV. TRYING TO DO THIS POROJECT IN NOTEBOOK AND IT WORKS FINE IN  CMD LINE PROMT WHEN USING VENV.
BUT HOWEVER, WHEN I DONT USE VN]ENV AND WRITE FLASK RUN IT SAYS FLASK NOT FOUND, EVEN THOUGH ITS INSTALLED 
AHHHHHHHHHHHHHHHHHHHHHHHHHHHHHHHHHHHHHHHHHHHHHHHHHHHHHHHHHHHHHHHHHHHHHHHHHHHHHHHHHHHHHHHHHHHHHHHHHHHHHHHHHHHHHHHHHHHHHHHHHHHHHHHHHHHHHHHHHHHHHHHHHHHHHHHHHHHHHHHHHHHHHHHHHHHHHHHHHHHHHHHHHHHHHHHHHHHHHHHHHHHHHHHHHHHHHHHHHHHHHHHHHHH




helpful refs
    the mosty important flask mega tutorial: [https://blog.miguelgrinberg.com/post/the-flask-mega-tutorial-part-i-hello-world]
    terminal shortcuts [https://gist.github.com/austinlparker/9202767]
    cmd line, pip documentation [https://pip.pypa.io/en/stable/cli/pip_list/]
    classic github keyboard shortcut [https://github.com/Maximinodotpy/articles/blob/main/039%20-%20Keyboard%20Shortcuts/shortcut-data/shortcuts.json] for aft chnaging keyboard to reset all to orginal go to keyboard shortcut json files copy paste this code there
    what does config mean [https://www.red-dove.com/old_site/config-doc/] [https://docs.red-dove.com/cfg/index.html]
    **in this code config meant noth,have changed the code to "from tom import TOM", and changed config.py to tom.py to resuce confusion with the module Config used with cfg files.
    
    
whats config in "from config import Config"?
    it means nothing in this code config meant noth, from config meant from file config.py and Config is name of class in config.py, have changed the code to from tom import TOM, and changed config.py to tom.py to resuce confusion with the module Config used with cfg files

the entire config file is actuallly uneccessary, it could be wriitte  as such in the main app.py file
            app = Flask(__name__)
            app.config['SECRET_KEY'] = 'you-will-never-guess'
            # ... add more variables here as needed
however in order fior more cleanliness they put it(the variables such as secret key) into a config.py or tom.py file. Then they connected it into the app.py file using config.from_object function
            app = Flask(__name__)
            app.config.from_object(TOM)


what config means in normal coding
            config is a python module that used to be called and is updated from  configparser, thus in w3s and online it is stil called         configparser, but a bit more compliacted than that. 
        1.its meant to be used with a cfg file, like this 
            .cfg file: langs.cfg
                        nested: {
                          hello: {
                            en: 'Hello'
                            fr: 'Bonjour'
                            de: 'Hallo'
                            es: 'Hola'
                          }
                          goodbye: {
                            en: 'Goodbye'
                            fr: 'Au revoir'
                            de: 'Auf Wiedersehen'
                            es: 'Adiós'
                          }
                        }
                        #You can access the nested values in a single step. Again, using Python, we have:
            .py file:
                        from config import Config
                        cfg = Config('langs.cfg')
                        print(cfg['nested.hello.es'])  # will print 'Hola'
                        print(cfg['nested.goodbye.fr'])  # will print 'Au revoir'
           Note that theres 2 files a .cfg file with the messgae(info), and .py file. example can be seen here [https://docs.red-dove.com/cfg/index.html]
           
   

cmd line advanced tips
    pip list shows everything installed but flask is onnly shown in the list if inside ven, outside venv its not there. that means flask is installed inside venv and not outside
    
cmd line,notepad and file explorer basic 
    #cmd line is opened by typing cmd into the directory bar of file explorer#
    cmd line acts as a terminal(same way in github), notepad acts as a script
    cmd line trick, cd changes the directory type: cd pathway
          **running html file** - write in notepad, save it as .html, right click file in file expl and run in chrome
          **running python file** in notepad, save as .py in file explorer, open file explorer and type cmd into the directory bar it willl open a   programming window themn jst type: python name of file   [https://www.youtube.com/watch?v=l4LODI94Jp4] 
          **combining html and python**?????????????????? django jinja
    running virtual environment [https://www.w3schools.com/Python/python_virtualenv.asp]using venv function creates a whole new folder in file explorer in with choosable name for programming, then in the same directory(not inside), can create a noteook python file but instead of running it by itself in the cmd line termainal, can run it within the virtual env within cmf line terminal, helps wiht installling modules

combing html and python
    jinja is being used here. it is inbuilt into flask 
    
TEMPLATES 
{block content}{end block} is put in base.html so later when base. html is extended this place is where the next html template is inserted into
https://blog.miguelgrinberg.com/post/the-flask-mega-tutorial-part-ii-templates

Database and models 
USERS is a class in models.py that is used to store all the data
db is where data ius stored and is defined in init.py as db = SQLAlchemy(app)
db.session.add is used in registration to update the databse with the new user, password
    user = db.session.scalar(sa.select(USER).where(USER.username == username.data)),   
    if user is not None:
                raise ValidationError('Please use a different username.')
    ###picks out any username in the USER class stores that is same as username data jst enetered in form, to cjeck if person is alrdy signed up.###
******THE WHOLE PART OF FUNCTION OF ADDING THE USERNAME TO THE DATATBASE IS UNDER IF FORM.VALIDATE ON SUBMIT SECTION IN ROUTES.PY******
******The form.validate_on_submit() method does all the form processing work******

Registration 
MADE USER SIGNUP POSSIBLE WOOHOOOOO, TO SEE THE USERS LOGGED IN.... IN PYTHON TERMINL WRITE
    
    >>> from app import app, db
    >>> from app.models import User, Post
    >>> import sqlalchemy as sa
    >>> app.app_context().push()
    
    ##method 1 
    >>> users = db.session.scalars(sa.select(User)).all()
    >>> users
    
    ##method 2 
    >>> users = db.session.scalars(query)
    >>> for u in users:
    ...     print(u.id, u.username)
    ...


how does login form work
@app.route('/login', methods=['GET', 'POST'])
def login():
    if current_user.is_authenticated:
        return redirect(url_for('index'))
    form = LoginForm()
    if form.validate_on_submit():
        user = db.session.scalar(
            sa.select(User).where(User.username == form.username.data))
        if user is None or not user.check_password(form.password.data):
            flash('Invalid username or password')
            return redirect(url_for('login'))
        login_user(user, remember=form.remember_me.data)
        return redirect(url_for('index'))
    return render_template('login.html', title='Sign In', form=form)

if user is None or not user.check_password(form.password.data): user is none is obvious. user.check_password() returns either a false or true option. so if password false, then false is returned, if not false equals to if true(aka known as if 1=1=2) and thus the program inside automaticaly runs
        
New addition:
adding messages so that ust becomes a messaging platfor
2 things:
    1. making posts public(on everyones feed or profile page), being able to follow frens and see their posts 
    2. direct messages from on person to another

1. so basically using "all_users = db.session.scalars(sa.select(USER)).all()" in users.py to call out all users and
        {% for user in user_list %}
        <p>{{ user.username }} </p>
        {% endfor %}
i have been able to get a list of all users in tnto profile.html, now i want to make them into links to theor profiles. sidenote: writing {{ user.username }} instead of <p>{{ user.username }} </p>, u get a single luine of usernames instead of a list
    adding links, able to add, huge sexurity issue, links go to their profile page where they can change theior about me and updfate view. have to create one more profilepage jst for viewing. created it called profile_for_view, route also changed to go to anoither link called view_user/username
                



