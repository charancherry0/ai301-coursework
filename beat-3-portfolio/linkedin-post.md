This term I built an AI assistant that searches GitHub for beginner-friendly open source issues and checks whether each one is a good fit for me. Its first run picked the wrong issues, so I rewrote its rules, and it led me to a real issue in the French government's open-source AI chat app.

The first run taught me something: most "good first issues" are already taken. Nine of the eleven I checked by hand already had someone's fix waiting for review, and my tool only noticed after I had paid for it to check them. I changed the search so it skips taken issues up front, and I fixed four of its rules where real projects had fooled it.

The improved search found an issue in Conversations asking the app to identify itself when it calls AI services. My tool flagged that the exact format wasn't agreed yet, so I claimed the issue and asked the maintainers to confirm the format before I write any code. I'm still waiting for their reply.

What I learned: a tool's first run is a test, not an answer, and a clear question is often the most useful first contribution.

https://github.com/suitenumerique/conversations/issues/911#issuecomment-6812019713

#OpenSource #GenerativeAI #Python
