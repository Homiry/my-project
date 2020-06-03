<template>
 <div class="wrapper" >
    <ul class="content"> 
      <button @click="btnClick">按钮</button>
      <li>fenle1</li>
      <li>fenle2</li>
      <li>fenle3</li>
      <li>fenle4</li>
      <li>fenle5</li>
      <li>fenle6</li>
      <li>fenle7</li>
      <li>fenle8</li>
      <li>fenle9</li>
      <li>fenle10</li>
      <li>fenle11</li>
      <li>fenle12</li>
      <li>fenle13</li>
      <li>fenle14</li>
      <li>fenle15</li>
      <li>fenle16</li>
      <li>fenle17</li>
      <li>fenle18</li>
      <li>fenle19</li>
      <li>fenle20</li>
      <li>fenle21</li>
      <li>fenle22</li>
      <li>fenle23</li>
      <li>fenle24</li>
      <li>fenle25</li>
      <li>fenle26</li>
      <li>fenle27</li>
      <li>fenle28</li>
      <li>fenle29</li>
      <li>fenle30</li>
      <li>fenle31</li>
      <li>fenle32</li>
      <li>fenle33</li>
      <li>fenle34</li>
      <li>fenle35</li>
      <li>fenle36</li>
      <li>fenle37</li>
      <li>fenle38</li>
      <li>fenle39</li>
      <li>fenle40</li>
      <li>fenle41</li>
      <li>fenle42</li>
      <li>fenle43</li>
      <li>fenle44</li>
      <li>fenle45</li>
      <li>fenle46</li>
      <li>fenle47</li>
      <li>fenle48</li>
      <li>fenle49</li>
      <li>fenle50</li>
      <li>fenle51</li>
      <li>fenle52</li>
      <li>fenle53</li>
      <li>fenle54</li>
      <li>fenle55</li>
      <li>fenle56</li>
      <li>fenle57</li>
      <li>fenle58</li>
      <li>fenle59</li>
      <li>fenle60</li>
      <li>fenle61</li>
      <li>fenle62</li>
      <li>fenle63</li>
      <li>fenle64</li>
      <li>fenle65</li>
      <li>fenle66</li>
      <li>fenle67</li>
      <li>fenle68</li>
      <li>fenle69</li>
      <li>fenle70</li>
      <li>fenle71</li>
      <li>fenle72</li>
      <li>fenle73</li>
      <li>fenle74</li>
      <li>fenle75</li>
      <li>fenle76</li>
      <li>fenle77</li>
      <li>fenle78</li>
      <li>fenle79</li>
      <li>fenle80</li>
      <li>fenle81</li>
      <li>fenle82</li>
      <li>fenle83</li>
      <li>fenle84</li>
      <li>fenle85</li>
      <li>fenle86</li>
      <li>fenle87</li>
      <li>fenle88</li>
      <li>fenle89</li>
      <li>fenle90</li>
      <li>fenle91</li>
      <li>fenle92</li>
      <li>fenle93</li>
      <li>fenle94</li>
      <li>fenle95</li>
      <li>fenle96</li>
      <li>fenle97</li>
      <li>fenle98</li>
      <li>fenle99</li>
      <li>fenle100</li>
    </ul>
 </div>
</template>

<script>
  import BScroll from 'better-scroll'

  export default {
    name: "Category",
    data() {
      return {
        scroll: null
      }
    },
    mounted() {
      this.scroll = new BScroll(document.querySelector('.wrapper'), {
        probeType: 3,
        pullUpLoad: true,
      })

      this.scroll.on('scroll', (position) => {
        console.log(position);
      })

      this.scroll.on('pullingUp', () => {
        console.log('加载更多')
      })
    },
    methods: {
      btnClick() {
        console.log('btnClick')
      }
    }
  }
</script>

<style scoped>
  .wrapper {
    height: 150px;
    background-color: #f00;
    overflow: hidden;
    /* overflow-y: scroll; */
  }
</style>
