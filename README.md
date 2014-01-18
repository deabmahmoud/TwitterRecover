My twitter account is hacked by someone few days ago.
He tweeted 500+ messages in Russian with links,
   and followed 300+ honeypots.

So I have to remove all my tweets and clean my following list.

First, [create an application](https://dev.twitter.com/docs) in Twitter, and use the scripts to get oauth. The script requires [requests](https://github.com/kennethreitz/requests/) along with [requests-oauthlib](https://github.com/requests/requests-oauthlib).

After you: `oauth = get_oauth()`
you can use 

    requests.get(url=get_url, auth=oauth)

or 

    request.post(url=post_url, auth=oauth, data=post_data)

to get and send message using Twitter API

Take me (@vancexu) as an example, the following code remove all my 300+ followings in 1 minutes.

    get_url = 'https://api.twitter.com/1.1/friends/ids.json?cursor=-1&screen_name=vancexu'
    r = requests.get(url=get_url, auth=oauth)
    res = r.json()
    ids = res['ids'] # get the ids of who I followed
    post_url = 'https://api.twitter.com/1.1/friendships/destroy.json'
    for id in ids:
        # each post request will delete one id I followed
        requests.post(url=post_url, auth=oauth, data={'user_id': str(id)}

The following code remove 200 of my tweets one time

    get_url = 'https://api.twitter.com/1.1/statuses/user_timeline.json?screen_name=vancexu&count=500'
    r = requests.get(url=get_url, auth=oauth)
    res = r.json()
    ids = []
    # get all tweets' ids
    for i in res:
        ids.append(i['id_str'])

    post_url = 'https://api.twitter.com/1.1/statuses/destroy/'
    for i in ids:
        # remove one tweet at a time, according to the given tweet id.
        requests.post(url=post_url+i+'.json', auth=oauth)

reference: http://thomassileo.com/blog/2013/01/25/using-twitter-rest-api-v1-dot-1-with-python/