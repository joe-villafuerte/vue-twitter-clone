<template>
  <div class="user-profile">
    <div class="user-profile__user-panel">
      <h1 class="user-profile__username">@{{ user.username }}</h1>
      <div class="user-profile__admin-badge" v-if="user.isAdmin">Admin</div>
      <div class="user-profile__follower-count">
        <strong>Followers:</strong> {{ followers }}
      </div>
      <form
        class="user-profile__create-tweet"
        @submit.prevent="createNewTweet"
        :class="{ exceeded: newTweetCharacterCount > 180 }"
      >
        <label for="newTweet"
          ><strong>New Tweet</strong> ({{ newTweetCharacterCount }}/180)</label
        >
        <textarea name="newTweet" rows="4" v-model="newTweetContent"></textarea>

        <div class="user-profile__create-tweet-type">
          <label for="newTweetType"><strong>Type:</strong></label>
          <select id="newTweetType" v-model="selectedTweetType">
            <option
              :value="option.value"
              v-for="(option, index) in tweetTypes"
              :key="index"
            >
              {{ option.name }}</option
            >
          </select>
        </div>
        <button>Send Tweet!!</button>
      </form>
    </div>
    <div class="user-profile__tweets-wrapper">
      <TweetItem
        v-for="tweet in user.tweets"
        :key="tweet.id"
        :username="user.username"
        :tweet="tweet"
        @favorite="toggleFavorite"
      />
    </div>
  </div>
</template>

<script>
import TweetItem from "./TweetItem";

export default {
  name: "UserProfile",
  components: { TweetItem },
  data() {
    return {
      newTweetContent: "",
      selectedTweetType: "instant",
      tweetTypes: [
        { value: "draft", name: "Draft" },
        { value: "instant", name: "Instant Tweet" },
      ],
      followers: 0,
      user: {
        id: 1,
        username: "_itsyourname",
        firstName: "Hey",
        lastName: "Villa",
        email: "joe@joe.com",
        isAdmin: true,
        tweets: [
          { id: 1, content: "Twitter is amazing" },
          { id: 2, content: "Twitter is super coooooool" },
        ],
      },
    };
  },
  watch: {
    followers(newFollowerCount, oldFollowerCount) {
      if (oldFollowerCount < newFollowerCount) {
        console.log(`${this.user.username} has a gained a follower!!`);
      }
    },
  },
  computed: {
    fullName() {
      return `${this.user.firstName} ${this.user.lastName}`;
    },
    newTweetCharacterCount() {
      return this.newTweetContent.length;
    },
  },
  methods: {
    followUser() {
      this.followers++;
    },
    toggleFavorite(id) {
      console.log(`favorited tweet #${id}`);
    },
    // Update tweet thread when user submits Instant Tweet
    // createNewTweet() {
    //   if (this.newTweetContent && this.selectedTweetType !== "draft") {
    //     this.user.tweets.unshift(
    //       items: {
    //         id: this.user.tweets.length + 1,
    //         content: this.newTweetContent
    //       })
    //         this.newTweetContent = ""
    //   }
    // },
  },
  mounted() {
    this.followUser();
  },
};
</script>

<style>
.user-profile {
  display: grid;
  grid-template-columns: 1fr 3fr;
  width: 100%;
  padding: 50px 5%;
}

.user-profile__user-panel {
  display: flex;
  flex-direction: column;
  margin-right: 50px;
  padding: 20px;
  background-color: white;
  border-radius: 5px;
  border: 1px solid #dfe3e8;
}

.user-profile__admin-badge {
  background: red;
  color: white;
  border-radius: 5px;
  border: 1px solid #dfe3e8;
  padding: 0 10px;
  font-weight: bold;
  text-align: center;
  width: 50px;
}

h1 {
  margin: 0;
}

.exceeded {
  color: red;
  background: red;
}

.user-profile__tweets-wrapper {
  display: grid;
  grid-gap: 10px;
}

.user-profile__create-tweet {
  padding-top: 20px;
  display: flex;
  flex-direction: column;
}
</style>
