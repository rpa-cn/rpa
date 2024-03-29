---
title: 还有那些人在关注 RPA
type: guide
order: 803
---

{% raw %}
<script id="vuer-profile-template" type="text/template">
  <div class="vuer">
    <div class="avatar">
      <img v-if="profile.github"
        :src="'https://github.com/' + profile.github + '.png'"
        :alt="profile.name" width=80 height=80>
    </div>
    <div class="profile">
      <h3 :data-official-title="profile.title">
        {{ profile.name }}
        <sup v-if="profile.title && titleVisible" v-html="profile.title"></sup>
      </h3>
      <dl>
        <template v-if="profile.reposOfficial">
          <dt>关注领域</dt>
          <dd>
            <ul>
              <li v-for="repo in profile.reposOfficial">
                <a :href="githubUrl('vuejs', repo)" target=_blank>{{ repo.name || repo }}</a>
              </li>
            </ul>
          </dd>
        </template>
        <template v-if="profile.github && profile.reposPersonal">
          <dt>Ecosystem</dt>
          <dd>
            <ul>
              <li v-for="repo in profile.reposPersonal">
                <a :href="githubUrl(profile.github, repo)" target=_blank>{{ repo.name || repo }}</a>
              </li>
            </ul>
          </dd>
        </template>
        <template v-if="profile.work">
          <dt>
            <i class="fa fa-briefcase"></i>
            <span class="sr-only">Work</span>
          </dt>
          <dd v-html="workHtml"></dd>
        </template>
        <span v-if="profile.distanceInKm" class="distance">
          <dt>
            <i class="fa fa-map-marker"></i>
            <span class="sr-only">Distance</span>
          </dt>
          <dd>
            About
            <span
              v-if="profile.distanceInKm <= 150"
              :title="profile.name + ' is close enough to commute to your location.'"
              class="user-match"
            >{{ textDistance }} away</span>
            <template v-else>{{ textDistance }} away</template>
            in {{ profile.city }}
          </dd>
        </span>
        <template v-else-if="profile.city">
          <dt>
            <i class="fa fa-map-marker"></i>
            <span class="sr-only">City</span>
          </dt>
          <dd>
            {{ profile.city }}
          </dd>
        </template>
        <template v-if="profile.languages">
          <dt>
            <i class="fa fa-globe"></i>
            <span class="sr-only">Languages</span>
          </dt>
          <dd v-html="languageListHtml" class="language-list"></dd>
        </template>
        <template v-if="profile.links">
          <dt>
            <i class="fa fa-link"></i>
            <span class="sr-only">Links</span>
          </dt>
          <dd>
            <ul>
              <li v-for="link in profile.links">
                <a :href="link" target=_blank>{{ minimizeLink(link) }}</a>
              </li>
            </ul>
          </dd>
        </template>
        <footer v-if="hasSocialLinks" class="social">
          <a class=github v-if="profile.github" :href="githubUrl(profile.github)">
            <i class="fa fa-github"></i>
            <span class="sr-only">Github</span>
          </a>
          <a class=twitter v-if="profile.twitter" :href="'https://twitter.com/' + profile.twitter">
            <i class="fa fa-twitter"></i>
            <span class="sr-only">Twitter</span>
          </a>
          <a class=codepen v-if="profile.codepen" :href="'https://codepen.io/' + profile.codepen">
            <i class="fa fa-codepen"></i>
            <span class="sr-only">CodePen</span>
          </a>
          <a class=weibo v-if="profile.weibo" :href="'https://weibo.com/' + profile.weibo">
            <i class="fa fa-weibo"></i>
            <span class="sr-only">微博</span>
          </a>
          <a class=zhihu v-if="profile.zhihu" :href="'https://www.zhihu.com/people/' + profile.zhihu">
            <i class="fa fa-zhihu"></i>
            <span class="sr-only">知乎</span>
          </a>
          <a class=weixin v-if="profile.weixin" :href="'https://weixin.com/' + profile.weixin">
            <i class="fa fa-weixin"></i>
            <span class="sr-only">微信</span>
          </a>          
        </footer>
      </dl>
    </div>
  </div>
</script>

<div id="team-members">
  <div class="team">

    <h2 id="the-core-team">
      实战派
      <button
        v-if="geolocationSupported && !userPosition"
        @click="getUserPosition"
        :disabled="isSorting"
        class="sort-by-distance-button"
      >
        <i
          v-if="isSorting"
          class="fa fa-refresh rotating-clockwise"
        ></i>
        <template v-else>
          <i class="fa fa-map-marker"></i>
          <span>find near me</span>
        </template>
      </button>
    </h2>

    <p v-if="errorGettingLocation" class="tip">
      Failed to get your location.
    </p>

    <p>
      The development of Vue and its ecosystem is guided by an international team, some of whom have chosen to be featured below.
    </p>

    <p v-if="userPosition" class="success">
      The core team has been sorted by their distance from you.
    </p>

    <vuer-profile
      v-for="profile in sortedTeam"
      :key="profile.github"
      :profile="profile"
      :title-visible="titleVisible"
    ></vuer-profile>
  </div>

  <div class="team">
    <h2 id="community-partners">
      站台帮
      <button
        v-if="geolocationSupported && !userPosition"
        @click="getUserPosition"
        :disabled="isSorting"
        class="sort-by-distance-button"
      >
        <i
          v-if="isSorting"
          class="fa fa-refresh rotating-clockwise"
        ></i>
        <template v-else>
          <i class="fa fa-map-marker"></i>
          <span>find near me</span>
        </template>
      </button>
    </h2>

    <p v-if="errorGettingLocation" class="tip">
      Failed to get your location.
    </p>

    <p>
      Some members of the Vue community have so enriched it, that they deserve special mention. We've developed a more intimate relationship with these key partners, often coordinating with them on upcoming features and news.
    </p>

    <p v-if="userPosition" class="success">
      The community partners have been sorted by their distance from you.
    </p>

    <vuer-profile
      v-for="profile in sortedPartners"
      :key="profile.github"
      :profile="profile"
      :title-visible="titleVisible"
    ></vuer-profile>
  </div>
</div>

<script>
(function () {
  var cityCoordsFor = {
    'Annecy, France': [45.899247, 6.129384],
    'Alicante, Spain' : [38.346543, -0.483838],
    'Bangalore, India': [12.971599, 77.594563],
    'Beijing, China': [39.904200, 116.407396],
    'Bordeaux, France': [44.837789, -0.579180],
    'Bucharest, Romania': [44.426767, 26.102538],
    'Chengdu, China': [30.572815, 104.066801],
    'Chongqing, China': [29.431586, 106.912251],
    'Denver, CO, USA': [39.739236, -104.990251],
    'Dubna, Russia': [56.732020, 37.166897],
    'East Lansing, MI, USA': [42.736979, -84.483865],
    'Hangzhou, China': [30.274084, 120.155070],
    'Jersey City, NJ, USA': [40.728157, -74.558716],
    'Kingston, Jamaica': [18.017874, -76.809904],
    'Krasnodar, Russia': [45.039267, 38.987221],
    'Lansing, MI, USA': [42.732535, -84.555535],
    'London, UK': [51.507351, -0.127758],
    'Lyon, France': [45.764043, 4.835659],
    'Mannheim, Germany': [49.487459, 8.466039],
    'Moscow, Russia': [55.755826, 37.617300],
    'Munich, Germany': [48.137154, 11.576124],
    'Orlando, FL, USA': [28.538335, -81.379236],
    'Paris, France': [48.856614, 2.352222],
    'China': [52.4006553, 16.761583],
    'Seoul, South Korea': [37.566535, 126.977969],
    'Shanghai, China': [31.230390, 121.473702],
    'Taquaritinga, Brazil': [-21.430094, -48.515285],
    'Tehran, Iran': [35.689197, 51.388974],
    'Thessaloniki, Greece': [40.640063, 22.944419],
    'Tokyo, Japan': [35.689487, 139.691706],
    'Toronto, Canada': [43.653226, -79.383184],
    'Wrocław, Poland': [51.107885, 17.038538]
  }
  var languageNameFor = {
    en: 'English',
    zh: '中文',
    vi: 'Tiếng Việt',
    pl: 'Polski',
    pt: 'Português',
    ru: 'Русский',
    jp: '日本語',
    fr: 'Français',
    de: 'Deutsch',
    el: 'Ελληνικά',
    es: 'Español',
    hi: 'हिंदी',
    fa: 'فارسی',
    ko: '한국어',
    ro: 'Română'
  }

  var team = [
  {
    name: 'Saurav',
    title: '',
    city: 'India',
    languages: ['en'],
    weixin: '',
    github: '111',
    weibo: '',
    zhihu: '',
    work: {
      role: 'Senior Research Analyst',
      org: 'Edureka'
    },
    reposOfficial: [
      'Top Writer - RPA & UiPath Technology is a kind monster, I believe.'
    ],
    links: [
      'http://www.linkedin.com/in/saurav25',
      'https://www.facebook.com/prratt'
    ]
  },
  {
    name: 'Tolani Jaiye-Tikolo',
    title: '',
    city: 'Ireland',
    languages: ['en'],
    weixin: '',
    github: '111',
    weibo: '',
    zhihu: '',
    work: {
      role: 'Robotic Process Automation (RPA) Developer',
      org: 'Blue Prism'
    },
    reposOfficial: [
      'Top Writer - RPA'
    ],
    links: [
      'https://www.facebook.com/'
    ]
  },
    {
    name: 'Siyong Liu',
    title: '',
    city: 'Singapore',
    languages: ['en'],
    weixin: '',
    github: '111',
    weibo: '',
    zhihu: '',
    work: {
      role: 'Thought Leader in Robotic Process Automaton.',
      org: ''
    },
    reposOfficial: [
      'Top Writer - RPA & UiPath'
    ],
    links: [
      'https://www.cfb-bots.com/blog',
      'https://www.linkedin.com/in/siyong-liu-20027210/'
    ]
  }, 
    {
    name: 'Saurav',
    title: '',
    city: 'China',
    languages: ['en'],
    weixin: '',
    github: '111',
    weibo: '',
    zhihu: '',
    work: {
      role: 'Technology is a kind monster, I believe.',
      org: ''
    },
    reposOfficial: [
      'Top Writer - RPA & UiPath'
    ],
    links: [
      'http://www.linkedin.com/in/saurav25',
      'https://www.facebook.com/prratt'
    ]
  }, 
  {
    name: '中国安防展览网AFzhan',
    title: '',
    city: 'China',
    languages: ['zh'],
    weixin: 'afzhan2005',
    github: '111',
    weibo: 'afzhan',
    zhihu: 'liang-yigang',
    work: {
      role: '官方微博',
      org: '浙江兴旺宝明通网络有限公司'
    },
    reposOfficial: [
      '自动化'
    ],
    links: [
      'http://www.afzhan.com'
    ]
  }  

  ]

  team = team.concat(shuffle([
    {
      name: '任子旭',
      title: 'Good Word Putter-Togetherer',
      city: 'China',
      languages: ['zh'],
      github: '111',
      twitter: '',
      gongzhonghao: 'RPA2018',
      weixin: 'rrenzixu',     
      zhihu: 'ZackRen',            
      work: {
        role: 'IT咨询',
        org: '致同会计师事务所'
      },
      reposOfficial: [
       'RPA'
      ],
      links: [
        '13811054515@126.com'
      ]
    },
    {
      name: '梁一纲',
      city: 'China',
      languages: ['zh'],
      github: '111',
      twitter: '',
      gongzhonghao: '',
      weixin: 'cunama',     
      zhihu: 'liang-yigang',            
      work: {
        role: '咨询/流程优化/开发',
        org: '埃森哲'
      },
      reposOfficial: [
       'RPA'
      ],
      links: [
        ''
      ]
    },
    {
      name: '猫奴二胖子',
      city: 'China',
      languages: ['zh'],
      github: '111',
      twitter: '',
      gongzhonghao: '',
      weixin: '',     
      zhihu: 'erpangzier',            
      work: {
        role: '咨询/流程优化/开发',
        org: 'xx'
      },
      reposOfficial: [
       '职业猫奴'
      ],
      links: [
        ''
      ]
    },
    {
      name: '徐鹏程',
      city: 'China',
      languages: ['zh'],
      github: '111',
      twitter: '',
      gongzhonghao: '',
      weixin: '',     
      zhihu: 'erpangzier',            
      work: {
        role: 'CIO',
        org: 'HRIS'
      },
      reposOfficial: [
       'RPA'
      ],
      links: [
        'https://www.zhihu.com/people/xu-peng-cheng-72-14/activities'
      ]
    },
    {
      name: 'slice',
      city: 'China',
      languages: ['zh'],
      github: '111',
      twitter: '',
      gongzhonghao: '',
      weixin: '',     
      zhihu: 'pyj-8-53',            
      work: {
        role: '',
        org: ''
      },
      reposOfficial: [
       'RPA'
      ],
      links: [
        'https://www.zhihu.com/people/pyj-8-53/activities'
      ]
    },
    {
      name: '陈键',
      city: 'China',
      languages: ['zh'],
      github: '111',
      twitter: '',
      gongzhonghao: '',
      weixin: '',     
      zhihu: 'chen-jian-34',            
      work: {
        role: '键精灵（RPA）开发者',
        org: ''
      },
      reposOfficial: [
       'RPA'
      ],
      links: [
        'https://www.zhihu.com/people/chen-jian-34/activities'
      ]
    },
    {
      name: 'joseph',
      city: 'China',
      languages: ['zh'],
      github: '111',
      twitter: '',
      gongzhonghao: '',
      weixin: '',     
      zhihu: 'chen-jian-34',            
      work: {
        role: '外包程序狗',
        org: ''
      },
      reposOfficial: [
       'RPA'
      ],
      links: [
        'https://www.zhihu.com/people/chen-jian-34/activities'
      ]
}       


 ]))

  var partners = [
    {
      name: '折向东',
      city: 'China',
      languages: ['zh'],
      github: '111',
      twitter: '',
      gongzhonghao: '瞎说开发那些事',
      weixin: '',     
      zhihu: 'xdong-she',            
      work: {
        role: '软件开发工程师',
        org: 'HPE'
      },
      reposOfficial: [
       'RPA'
      ],
      links: [
        'https://www.zhihu.com/people/xdong-she/activities'
      ]
}  ]

  Vue.component('vuer-profile', {
    template: '#vuer-profile-template',
    props: {
      profile: Object,
      titleVisible: Boolean
    },
    computed: {
      workHtml: function () {
        var work = this.profile.work
        var html = ''
        if (work.orgUrl) {
          html += '<a href="' + work.orgUrl + '" target="_blank">'
          if (work.org) {
            html += work.org
          } else {
            this.minimizeLink(work.orgUrl)
          }
          html += '</a>'
        } else if (work.org) {
          html += work.org
        }
        if (work.role) {
          if (html.length > 0) {
            html = work.role + ' @ ' + html
          } else {
            html = work.role
          }
        }
        return html
      },
      textDistance: function () {
        var distanceInKm = this.profile.distanceInKm || 0
        if (this.$root.useMiles) {
          return roundDistance(kmToMi(distanceInKm)) + ' miles'
        } else {
          return roundDistance(distanceInKm) + ' km'
        }
      },
      languageListHtml: function () {
        var vm = this
        var nav = window.navigator
        if (!vm.profile.languages) return ''
        var preferredLanguageCode = nav.languages
          // The preferred language set in the browser
          ? nav.languages[0]
          : (
              // The system language in IE
              nav.userLanguage ||
              // The language in the current page
              nav.language
            )
        return (
          '<ul><li>' +
          vm.profile.languages.map(function (languageCode, index) {
            var language = languageNameFor[languageCode]
            if (
              languageCode !== 'en' &&
              preferredLanguageCode &&
              languageCode === preferredLanguageCode.slice(0, 2)
            ) {
              return (
                '<span ' +
                  'class="user-match" ' +
                  'title="' +
                    vm.profile.name +
                    ' can give technical talks in your preferred language.' +
                  '"' +
                '\>' + language + '</span>'
              )
            }
            return language
          }).join('</li><li>') +
          '</li></ul>'
        )
      },
      hasSocialLinks: function () {
        return this.profile.github || this.profile.twitter || this.profile.codepen || this.profile.weibo || this.profile.weixin  || this.profile.gongzhonghao
      }
    },
    methods: {
      minimizeLink: function (link) {
        return link
          .replace(/^https?:\/\/(www\.)?/, '')
          .replace(/\/$/, '')
          .replace(/^mailto:/, '')
      },
      /**
       * Generate a GitHub URL using a repo and a handle.
       */
      githubUrl: function (handle, repo) {
        if (repo && repo.url) {
          return repo.url
        }
        if (repo && repo.indexOf('/') !== -1) {
          // If the repo name has a slash, it must be an organization repo.
          // In such a case, we discard the (personal) handle.
          return (
            'https://github.com/' +
            repo.replace(/\/\*$/, '')
          )
        }
        return 'https://github.com/' + handle + '/' + (repo || '')
      }
    }
  })

  new Vue({
    el: '#team-members',
    data: {
      team: team,
      partners: shuffle(partners),
      geolocationSupported: false,
      isSorting: false,
      errorGettingLocation: false,
      userPosition: null,
      useMiles: false,
      konami: {
        position: 0,
        code: [38, 38, 40, 40, 37, 39, 37, 39, 66, 65]
      }
    },
    computed: {
      sortedTeam: function () {
        return this.sortVuersByDistance(this.team)
      },
      sortedPartners: function () {
        return this.sortVuersByDistance(this.partners)
      },
      titleVisible: function () {
        return this.konami.code.length === this.konami.position
      }
    },
    created: function () {
      var nav = window.navigator
      if ('geolocation' in nav) {
        this.geolocationSupported = true
        var imperialLanguageCodes = [
          'en-US', 'en-MY', 'en-MM', 'en-BU', 'en-LR', 'my', 'bu'
        ]
        if (imperialLanguageCodes.indexOf(nav.language) !== -1) {
          this.useMiles = true
        }
      }
      document.addEventListener('keydown', this.konamiKeydown)
    },
    beforeDestroy: function () {
      document.removeEventListener('keydown', this.konamiKeydown)
    },
    methods: {
      getUserPosition: function () {
        var vm = this
        var nav = window.navigator
        vm.isSorting = true
        nav.geolocation.getCurrentPosition(
          function (position) {
            vm.userPosition = position
            vm.isSorting = false
          },
          function (error) {
            vm.isSorting = false
            vm.errorGettingLocation = true
          },
          {
            enableHighAccuracy: true
          }
        )
      },
      sortVuersByDistance: function (vuers) {
        var vm = this
        if (!vm.userPosition) return vuers
        var vuersWithDistances = vuers.map(function (vuer) {
          var cityCoords = cityCoordsFor[vuer.city]
          return Object.assign({}, vuer, {
            distanceInKm: getDistanceFromLatLonInKm(
              vm.userPosition.coords.latitude,
              vm.userPosition.coords.longitude,
              cityCoords[0],
              cityCoords[1]
            )
          })
        })
        vuersWithDistances.sort(function (a, b) {
          return (
            a.distanceInKm -
            b.distanceInKm
          )
        })
        return vuersWithDistances
      },
      konamiKeydown: function (event) {
        if (this.titleVisible) {
          return
        }

        if (event.keyCode !== this.konami.code[this.konami.position++]) {
          this.konami.position = 0
        }
      }
    }
  })

  /**
  * Shuffles array in place.
  * @param {Array} a items The array containing the items.
  */
  function shuffle (a) {
    a = a.concat([])
    if (window.location.hostname === 'localhost') {
      return a
    }
    var j, x, i
    for (i = a.length; i; i--) {
      j = Math.floor(Math.random() * i)
      x = a[i - 1]
      a[i - 1] = a[j]
      a[j] = x
    }
    return a
  }

  /**
  * Calculates great-circle distances between the two points – that is, the shortest distance over the earth’s surface – using the Haversine formula.
  * @param {Number} lat1 The latitude of the 1st location.
  * @param {Number} lon1 The longitute of the 1st location.
  * @param {Number} lat2 The latitude of the 2nd location.
  * @param {Number} lon2 The longitute of the 2nd location.
  */
  function getDistanceFromLatLonInKm(lat1,lon1,lat2,lon2) {
    var R = 6371 // Radius of the earth in km
    var dLat = deg2rad(lat2-lat1)  // deg2rad below
    var dLon = deg2rad(lon2-lon1)
    var a =
      Math.sin(dLat/2) * Math.sin(dLat/2) +
      Math.cos(deg2rad(lat1)) * Math.cos(deg2rad(lat2)) *
      Math.sin(dLon/2) * Math.sin(dLon/2)
    var c = 2 * Math.atan2(Math.sqrt(a), Math.sqrt(1-a))
    var d = R * c // Distance in km
    return d
  }

  function deg2rad(deg) {
    return deg * (Math.PI/180)
  }

  function kmToMi (km) {
    return km * 0.62137
  }

  function roundDistance (num) {
    return Number(Math.ceil(num).toPrecision(2))
  }
})()
</script>
{% endraw %}
