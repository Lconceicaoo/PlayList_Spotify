

```python

import sys
import spotipy
import spotipy.util as util

artistasFile = open('play.txt', 'r')
artista = [x.strip('\n') for x in artistasFile.readlines()]

tracks = []

numeroArtistas = len(artista)

username = 'em4p85nrs3mv13muskxjx9f05'
scope = 'playlist-modify-public'
playlist_id = '3Tv2ytPih85Rq2ByUj5UPI'

token = util.prompt_for_user_token(username,
                                   scope,
                                   client_id='bb8ba1e6c66d4d479391c9fd1c92862b',
                                   client_secret='edf975a6eaba47a1bd6624fd36fc8e6d',
                                   redirect_uri='http://localhost:5050/callback')

if token:
    sp = spotipy.Spotify(auth=token)
    sp.trace = False

    for x in range(0, numeroArtistas):
        result = sp.search(artista[x], limit=2)
        for i, t in enumerate(result['tracks']['items']):
            tracks.append(str(t['id'].strip('u')))
            print("adicionando a track", t['id'], t['name'])
    while tracks:
        try:
            result = sp.user_playlist_add_tracks(username, playlist_id, tracks[:1])
        except:
            print("erro")
        tracks = tracks[1:]

else:
    print("Can't get token for", username)
