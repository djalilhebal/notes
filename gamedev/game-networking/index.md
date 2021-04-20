---
permalink: "/game-networking/"
---

# Game Networking
#Networking in #gamedev.

---

- [x] READ LIKED: [Building a Peer-to-Peer Multiplayer Networked Game](https://gamedevelopment.tutsplus.com/tutorials/building-a-peer-to-peer-multiplayer-networked-game--gamedev-10074)
    * KAITO: In ActionScript 3; non-authoritative peer-to-peer (P2P) approach.
    * [ ] save and archive the post!
    * [The final code is on GitHub](https://github.com/tutsplus/P2PMultiplayerGame) `[saved:@tutsplus__P2PMultiplayerGame]`

---

- ["How can I make a peer-to-peer multiplayer game?" | Game Development Stack Exchange](https://gamedev.stackexchange.com/questions/3887/how-can-i-make-a-peer-to-peer-multiplayer-game)
    * KAITO: Interesting quetion and answers.
    * TLDR: Difficult to synchronize: Huge data (message complexity), network lag, butterfly-effect style desynchronization; plus cheating.

---

- [x] READ LIKED: [What Every Programmer Needs To Know About Game Networking | Gaffer On Games](https://gafferongames.com/post/what_every_programmer_needs_to_know_about_game_networking/)
    * Glenn Fiedler
    * Feb 24, 2010
    * KAITO: Basically high-level descriptions of different approaches.
    * Networking models:
        + **Peer-to-Peer Lockstep**
          > The reason being that in RTS games the game state consists of many thousands of units and is simply too large to exchange between players.
          > These games have no choice but to exchange the commands which drive the evolution of the game state.
        + **Client/Server**
          Client as a _dumb terminal_ for their playable character/object.
        + **Client-Side Prediction**
          Described exactly as in [that Overwatch talk (GDC)](???).

---

- [ ] READ LONG: [Implementation of a Peer-to-Peer Multiplayer Game with ... - DVS](https://www.yumpu.com/en/document/read/48960342/implementation-of-a-peer-to-peer-multiplayer-game-with-dvs)
    `[saved:yumpu.com__document__48960342__implementation-of-a-peer-to-peer-multiplayer-game-with-dvs.pdf]`

---

END.
