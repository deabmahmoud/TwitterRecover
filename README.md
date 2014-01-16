My twitter account is hacked by someone few days ago.
He tweeted 500+ messages in Russian with links,
   and followed 300+ honeypots.

So I have to remove all my tweets and clean my following list.

First, setup the application in Twitter, and use the scripts to get oauth.

After you: `oauth = get_oauth()`
you do use 

    requests.get(url=get_url, auth=oauth)

or 

    request.post(url=post_url, auth=oauth, data=post_data)

to get and send message using Twitter API

Take me (@vancexu) as an example,

    get_url = 'https://api.twitter.com/1.1/friends/ids.json?cursor=-1&screen_name=vancexu'
    r = requests.get(url=get_url, auth=oauth)
    res = r.json()
    ids = res['ids'] # get the ids of who I followed
    post_url = 'https://api.twitter.com/1.1/friendships/destroy.json'
    # each post request will delete one id I followed
    for id in ids:
        requests.post(url=post_url, auth=oauth, data={'user_id': str(id)}


