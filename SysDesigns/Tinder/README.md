![Tinder](https://github.com/PriyankaKhire/SystemDesign/assets/12015512/12b98d15-dab7-4aff-a9a4-b64a6262f309)
  
<h2>Requirements</h2>

<h3>Functional</h3>

1. User should be able to create their Tinder profile by adding their bio and uploading photos.
2. Users should be able to view other users nearby, based on their geographic region.
3. Users should be able to like or dislike other users
4. Users should get notifications when matched

<h3>Non-functional</h3>

1. When the person logs in, they need to be able to see the recommended users with as little latency as possible.
2. The system should be highly available.

<h2>Capacity Estimations</h2>

- Storage: 
  - 50 million members
    - Assume each member meta data record is 100KB then to store member data we need 50million * 100Kb = 5TB of storage space
    - Assume we allow 10 pics of max size 3MB each per profile, we need 50million * 10 * 3MB = 187.5 TB of storage for media.
- Reads: Reads would be in the form of showing our users the recommended profiles in their geographic region. The reads depend on daly active users and how many maximum profiles they are shown a day
  - Assume we have 1million daly active users and at max each user is shown 100 profiles
    - then each user is reading = 100 profiles * (user metadata 100KB + pics (3MB * 10)) = 3.01GB per day
    - 1million users are reading a total of 3.01PB a day
- Writes:
  - Writes can be in the form of
    - user sign ups
    - users updating their profiles
      
This is a read heavy system. (Since we are not considering chatting as part of this design)

<h2>User Profile Creation</h2>

- Responsible for creating user profiles.
- Stores user information in a database.
- Adds users to a geo-sharded index to enable nearby user recommendations.
<h3>High level diagram</h3>

<h3>API design</h3>
<h3>Component design</h3>

<h1>Good Reads</h1>

- [Designing Tinder - High scalability blog](https://highscalability.com/designing-tinder/)






