---
order: 2
zh-CN:
  title: 受控模式
en-US:
  title: Controlled Mode
---

```jsx
import { Upload, Notify } from 'zent';

class Simple extends React.Component {
	state = {
		fileList: [],
	}

	onUploadChange = (files) => {
		console.log(files);
		this.setState({
			fileList: files,
		})
	}

	onUpload = (file, report) => {
		console.log('onUpload', file)
		return new Promise((resolve, reject) => {
			let count = 0;
			const update = () => {
				if (count < 100) {
					count += 5;
					report(count);
					setTimeout(update, 500);
				} else {
					// 随机成功或失败
					const success = Math.random() > 0.5;
					if (success) {
						resolve();
					} else {
						reject();
					}
				}
			}
			setTimeout(update, 500);
		})
	}

	onUploadError = (type, data) => {
		Notify.error(`错误类型: ${type}, 错误参数: ${JSON.stringify(data)}`)
	}

	render() {
		const { fileList } = this.state;
		return (
			<Upload
				multiple
				fileList={fileList}
				className="zent-upload-demo-pic"
				maxSize={2 * 1024 * 1024}
				maxAmount={3}
				onChange={this.onUploadChange}
				onUpload={this.onUpload}
				onError={this.onUploadError}
			/>
		);
	}
}

ReactDOM.render(<Simple />, mountNode);
```