# coding: utf-8
import requests
import json

class FacebookUtil():

    ############################################################################################################################
    ## ATTENTION
    ############################################################################################################################
    ##
    ## To make this work, you need a token which you can obtain from Graph API Explorer with the appropriate permissions.
    ## https://developers.facebook.com/tools/explorer/
    ##
    ############################################################################################################################

    TOKEN = 'YOUR_TOKEN'

    def get_posts_on_my_stream(self):
        """ list stream. you will need read_stream, read_insights permitions"""
        query = "SELECT post_id, actor_id, message FROM stream WHERE filter_key = 'owner' AND source_id = me() LIMIT 100000"
        return self.execute_fql_query(query)

    def smart_search(self, word):
        """ search on the wall. you will need read_stream, read_insights permitions"""
        print 'searching for %s ...\n' % word
        query  = "SELECT post_id, actor_id, message, share_count FROM stream WHERE source_id = me() AND strpos(message, '%s') >= 0 limit 100000" % word
        return self.execute_fql_query(query)

    def python_search(self, word):
        """ search on the wall """
        print 'searching for %s ...\n' % word
        posts  = self.get_posts_on_my_stream()
        result = [ x for x in posts if word in x['message']]
        self.print_messages(result)

    def reply(self, posts, message):
        """
            reply posts with a message. you will need the publish_stream permition
            you will need this on your birthday ;)
        """
        for post in posts:
            r = requests.get('https://graph.facebook.com/%s' % post['actor_id'])
            url = 'https://graph.facebook.com/%s/comments' % post['post_id']
            user = json.loads(r.text)
            payload = {'access_token': self.TOKEN, 'message': message}
            s = requests.post(url, data=payload)
            if s.status_code == 200:
                print 'done for %s' % post['post_id']
            else:
                print 'error for %s' % post['post_id']

    def print_messages(self, posts):
        """ print messages """
        for post in posts:
            print '=*='*50
            print post['share_count']
            print post['message'].encode('utf-8')

    def execute_fql_query(self, query):
        """ execute fql query """
        p = {'q': query, 'access_token': self.TOKEN}
        r = requests.get('https://graph.facebook.com/fql', params=p)
        result = json.loads(r.text)
        return result['data']

if __name__ == '__main__':
    fbu = FacebookUtil()
    posts = fbu.smart_search('ma nostalgia dos nossos arranca ra')
    fbu.print_messages(posts)
    #fbu.reply(posts, 'teste 123')

