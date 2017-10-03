<template>
  <div>
    <v-layout>
      <v-flex xs4>
        <panel title="Song Metadata">
            <v-text-field
              name="title"
              label="Title"
              required
              :rules="[required]"
              v-model="song.title"
            ></v-text-field>

            <v-text-field
              name="artist"
              label="Artist"
              required
              :rules="[required]"
              v-model="song.artist"
            ></v-text-field>

            <v-text-field
              name="genre"
              label="Genre"
              required
              :rules="[required]"
              v-model="song.genre"
            ></v-text-field>

            <v-text-field
              name="album"
              label="Album"
              required
              :rules="[required]"
              v-model="song.album"
            ></v-text-field>

            <v-text-field
              name="albumImage"
              label="Album Image"
              required
              :rules="[required]"
              v-model="song.albumImage"
            ></v-text-field>

            <v-text-field
              name="youtubeId"
              label="Youtube Id"
              required
              :rules="[required]"
              v-model="song.youtubeId"
            ></v-text-field>
        </panel>
      </v-flex>
      <v-flex>
        <panel title="Song Structure" class="ml-2">
          <v-text-field
            name="lyrics"
            label="Lyrics"
            required
            :rules="[required]"
            multi-line
            v-model="song.lyrics"
          ></v-text-field>

          <v-text-field
            name="tab"
            label="Tab"
            required
            :rules="[required]"
            multi-line
            v-model="song.tab"
          ></v-text-field>

          <div class="error" v-if="error">
            {{ error }}
          </div>
          <v-btn
            dark
            class="cyan"
            @click="save">
            Save Song
          </v-btn>
        </panel>
      </v-flex>
    </v-layout>
  </div>
</template>

<script>
import Panel from '@/components/Panel'
import SongsService from '@/services/SongsService'

export default {
  data () {
    return {
      song: {
        title: null,
        artist: null,
        genre: null,
        album: null,
        albumImage: null,
        youtubeId: null,
        lyrics: null,
        tab: null
      },
      error: null,
      required: (value) => !!value || 'Required.'
    }
  },
  methods: {
    async save () {
      this.error = null

      const areAllFieldsFilledIn = Object
        .keys(this.song)
        .every(key => !!this.song[key])

      if (!areAllFieldsFilledIn) {
        this.error = 'Please fill in all the required fields'
        return
      }

      const songId = this.$store.state.route.params.songId

      try {
        await SongsService.put(this.song)

        this.$router.push({
          name: 'song',
          params: {
            songId: songId
          }
        })
      } catch (e) {
        console.log(e)
      }
    }
  },
  async mounted () {
    try {
      const songId = this.$store.state.route.params.songId
      this.song = (await SongsService.show(songId)).data
    } catch (e) {
      console.log(e)
    }
  },
  components: {
    Panel
  }
}
</script>

<style scoped>
</style>
