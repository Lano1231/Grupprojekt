<script>
  import VaruKorg from './VaruKorg.vue'
  import { mapGetters } from 'vuex'
  export default {
    components: { VaruKorg },
    data() {
      return {
        showManNav: false,
        showKviNav: false,
        showLogNav: false,
        searchQuery: '',
        categories: [
          'jeans för män',
          'jackor för män',
          'skjortor för män',
          'träningsoverall för män',
          'klänningar för kvinnor',
          'skor för kvinnor',
          'jackor för kvinnor',
          'jeans för kvinnor'
        ],
        showDropdown: false,
        filteredCategories: [],
        valdValuta: 'Välj valuta'
      }
    },
    computed: {
      ...mapGetters(['cart', 'total', 'cartCount', 'currentCurrency'])
    },

    methods: {
      searchCategory() {
        const query = this.searchQuery.toLowerCase()
        if (query === 'jeans för män') {
          this.$router.push('/jeans')
        } else if (query === 'jackor för män') {
          this.$router.push('/jackets')
        } else if (query === 'skjortor för män') {
          this.$router.push('/shirts')
        } else if (query === 'träningsoverall för män') {
          this.$router.push('/tracksuits')
        } else if (query === 'klänningar för kvinnor') {
          this.$router.push('/klänningar')
        } else if (query === 'skor för kvinnor') {
          this.$router.push('/skor')
        } else if (query === 'jackor för kvinnor') {
          this.$router.push('/womensjackets')
        } else if (query === 'jeans för kvinnor') {
          this.$router.push('/womensjeans')
        } else {
          console.log('Category not found')
        }
      },
      setCurrency() {
        this.$store.commit('SET_CURRENCY', this.valdValuta)
      },
      updateDropdown() {
        if (this.searchQuery.length > 0) {
          this.showDropdown = true
          this.filteredCategories = this.categories.filter((category) =>
            category.toLowerCase().includes(this.searchQuery.toLowerCase())
          )
        } else {
          this.showDropdown = false
          this.filteredCategories = []
        }
      },
      selectCategory(category) {
        this.searchQuery = category
        this.showDropdown = false
      },

      maleFunction() {
        this.showManNav = !this.showManNav
        this.showKviNav = false
      },
      kvinnaFunction() {
        this.showKviNav = !this.showKviNav
        this.showManNav = false
      },
      navFunction() {
        this.showLogNav = !this.showLogNav
      }
    }
  }
</script>
<!-- Referens Navbar : https://mdbootstrap.com/docs/standard/navigation/navbar/ -->
<template>
  <div class="bg-dark">
    <header>
      <div class="p-3 text-center bg-light border-bottom">
        <div class="container">
          <div class="row">
            <div
              class="col-md-4 d-flex justify-content-center justify-content-md-start mb-3 mb-md-0"
            >
              <a href="#!" class="ms-md-2">
                <img src="Logo.png" height="35" />
              </a>
              <div
                @click="$event.stopPropagation()"
                class="Man col-md-3 d-flex justify-content-center justify-content-md-start mb-3 mb-md-0"
              >
                <li class="nav-item me-2 me-lg-0 d-none d-md-inline-block">
                  <a class="nav-link" id="hover-man">
                    <b-button
                      @click="maleFunction()"
                      pill
                      variant="outline-secondary"
                      >Man</b-button
                    >
                  </a>
                </li>
              </div>
              <div
                @click="$event.stopPropagation()"
                class="col-md-2 w-auto my-autocol-md-1 d-flex justify-content-center justify-content-md-start mb-3 mb-md-0"
              >
                <li class="nav-item me-2 me-lg-0 d-none d-md-inline-block">
                  <a class="nav-link" id="hover-kvinna">
                    <b-button
                      @click="kvinnaFunction()"
                      pill
                      variant="outline-secondary"
                      >Kvinna</b-button
                    >
                  </a>
                </li>
              </div>
            </div>

            <div class="col-md-4 position-relative">
              <form
                @click="$event.stopPropagation()"
                class="d-flex input-group w-auto my-auto mb-3 mb-md-0"
                @submit.prevent="searchCategory"
              >
                <input
                  autocomplete="off"
                  type="search"
                  class="form-control rounded"
                  placeholder="Sök efter kategori"
                  v-model="searchQuery"
                  @input="updateDropdown"
                />
                <div
                  v-if="showDropdown && filteredCategories.length"
                  class="search-dropdown"
                >
                  <ul class="list-group">
                    <li
                      class="list-group-item"
                      style="cursor: pointer"
                      v-for="category in filteredCategories"
                      :key="category"
                      @click="selectCategory(category)"
                    >
                      {{ category }}
                    </li>
                  </ul>
                </div>
                <b-button squared variant="outline-secondary" type="submit"
                  >Sök</b-button
                >
              </form>
            </div>

            <div
              class="col-md-4 d-flex justify-content-center justify-content-md-end align-items-center"
            >
              <div>
                <select v-model="valdValuta" id="valuta" @change="setCurrency">
                  <option for="valdValuta">Välj valuta</option>
                  <option value="SEK">SEK</option>
                  <option value="EUR">EUR</option>
                  <option value="NOK">NOK</option>
                  <option value="DKK">DKK</option>
                </select>
                <p>Du har valt att handla med {{ currentCurrency }}</p>
              </div>
              <div class="d-flex">
                <div>
                  <b-dropdown
                    size="lg"
                    variant="link"
                    toggle-class="text-decoration-none"
                    no-caret
                  >
                    <template #button-content>
                      <img
                        src="https://cdn-icons-png.flaticon.com/512/263/263142.png"
                        height="22"
                        alt=""
                        loading="lazy"
                      />
                      <b-badge style="font-size: 10px" pill variant="danger">{{
                        cartCount
                      }}</b-badge>
                    </template>
                    <div>
                      <VaruKorg />
                    </div>
                  </b-dropdown>
                </div>

                <div>
                  <b-dropdown
                    size="lg"
                    variant="link"
                    toggle-class="text-decoration-none"
                    no-caret
                  >
                    <template #button-content>
                      <img
                        src="https://mdbootstrap.com/img/Photos/Avatars/img (31).jpg"
                        class="rounded-circle"
                        height="22"
                        alt=""
                        loading="lazy"
                      />
                    </template>
                    <b-dropdown-item :to="'/login'">Min Profil</b-dropdown-item>
                    <b-dropdown-item :to="'/register'"
                      >Registrera dig</b-dropdown-item
                    >
                    <b-dropdown-item :to="'/likes'"
                      >Gillade Produkter</b-dropdown-item
                    >
                    <b-dropdown-item href="#">Logga ut</b-dropdown-item>
                  </b-dropdown>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </header>
  </div>
  <header>
    <nav
      class="navbar navbar-expand-lg navbar-light bg-light"
      v-show="showKviNav"
    >
      <div class="container justify-content-center justify-content-md-between">
        <ul class="navbar-nav flex-row" @click="$event.stopPropagation()">
          <li class="nav-item me-2 me-lg-0">
            <a
              role="button"
              data-mdb-toggle="sidenav"
              data-mdb-target="#sidenav-1"
              class="btn shadow-0 p-0 me-3 nav-link"
              aria-controls="#sidenav-1"
              aria-haspopup="true"
              @click="kvinnaFunction"
            >
              <i class="fas fa-bars me-1" />
            </a>
          </li>
          <li class="nav-item me-2 me-lg-0 d-none d-md-inline-block">
            <RouterLink class="nav-link" to="/Womensjackets">Jackor</RouterLink>
          </li>
          <li class="nav-item me-2 me-lg-0 d-none d-md-inline-block">
            <RouterLink class="nav-link" to="/Womensjeans">Jeans</RouterLink>
          </li>
          <li class="nav-item me-2 me-lg-0 d-none d-md-inline-block">
            <RouterLink class="nav-link" to="/klänningar"
              >Klänningar</RouterLink
            >
          </li>
          <li class="nav-item me-2 me-lg-0 d-none d-md-inline-block">
            <RouterLink class="nav-link" to="/skor">Skor</RouterLink>
          </li>
        </ul>
      </div>
    </nav>

    <nav
      class="navbar navbar-expand-lg navbar-light bg-white"
      v-show="showManNav"
    >
      <div class="container justify-content-center justify-content-md-between">
        <ul class="navbar-nav flex-row" @click="$event.stopPropagation()">
          <li class="nav-item me-2 me-lg-0">
            <a
              role="button"
              data-mdb-toggle="sidenav"
              data-mdb-target="#sidenav-1"
              class="btn shadow-0 p-0 me-3 nav-link"
              aria-controls="#sidenav-1"
              aria-haspopup="true"
              @click="maleFunction"
            >
              <i class="fas fa-bars me-1" />
            </a>
          </li>
          <li class="nav-item me-2 me-lg-0 d-none d-md-inline-block">
            <RouterLink class="nav-link" to="/shirts">Skjortor</RouterLink>
          </li>
          <li class="nav-item me-2 me-lg-0 d-none d-md-inline-block">
            <RouterLink class="nav-link" to="/jackets">Jackor</RouterLink>
          </li>
          <li class="nav-item me-2 me-lg-0 d-none d-md-inline-block">
            <RouterLink class="nav-link" to="/jeans">Jeans</RouterLink>
          </li>
          <li class="nav-item me-2 me-lg-0 d-none d-md-inline-block">
            <RouterLink class="nav-link" to="/tracksuits"
              >Träningsoveraller</RouterLink
            >
          </li>
        </ul>
      </div>
    </nav>
  </header>
  <div v-if="$route.path === '/!'">
    <LandPage />
  </div>
</template>

<style scoped>
  .search-dropdown {
    position: absolute;
    z-index: 1;
    top: calc(100% + 5px);
    left: 0;
    width: 100%;
  }
</style>
