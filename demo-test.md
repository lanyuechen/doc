## Demo code with instant preview and jsfiddle integration  {docsify-ignore-all}

```jsx
/*react*/
<desc>
  Hello `world`
  * a
</desc>
<script>
  export default class Application extends React.Component {
    constructor(props) {
      super(props)
      this.state = {
        color: 'blue'
      }
    }

    run() {

    }

    render() {
      return (
        <div>
          hello word
          <button onClick={this.run()}>运行</button>
        </div>
      )
    }
  }
</script>
```