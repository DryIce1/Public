---
type: datascope
---

```datacorejsx
const svgCopy = (<svg xmlns="http://www.w3.org/2000/svg" width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="lucide lucide-clipboard-copy"><rect width="8" height="4" x="8" y="2" rx="1" ry="1"/><path d="M8 4H6a2 2 0 0 0-2 2v14a2 2 0 0 0 2 2h12a2 2 0 0 0 2-2v-2"/><path d="M16 4h2a2 2 0 0 1 2 2v4"/><path d="M21 14H11"/><path d="m15 10-4 4 4 4"/></svg>);

const commands = app.commands.commands;
const commandEntries = Object.entries(commands);

commandEntries.sort((a, b) => a[1].name.localeCompare(b[1].name));

return function View() {
	const [filter, setFilter] = dc.useState("");
	const [copiedId, setCopiedId] = dc.useState(null);

	const filteredCommands = commandEntries
		.filter(([key, obj]) => 
			obj.name.toLowerCase().includes(filter.toLowerCase()) ||
			obj.id.toLowerCase().includes(filter.toLowerCase())
	);

	const handleCopy = (id) => { 
		navigator.clipboard.writeText(id); 
		setCopiedId(id); 
		setTimeout(() => setCopiedId(null), 2000);
	};

	return (
		<div>
			<input
				type="text"
				placeholder="Search..."
				value={filter}
				onChange={(e) => setFilter(e.target.value)}
			/>
			
			<table>
				<thead>
					<tr>
						<th>Command Name</th>
						<th>Command Id</th>
						<th></th>
					</tr>
				</thead>
				<tbody>
					{filteredCommands.map(([key, obj]) => (
						<tr key={key}>
							<td>{obj.name}</td>
							<td>{obj.id}</td>
							<td style={{ position: "relative", overflow: "visible" }}>
								<i 
									className="copy-icon"
									onClick={() => handleCopy(obj.id)}
								>
									{svgCopy}
								</i>
								{copiedId === obj.id && (
									<div className="copied-popup">Copied!</div>
								)}
							</td>
						</tr>
					))}
				</tbody>
			</table>
			
			<style> {` 
				.custom-no-outline {
					box-shadow:
				}
				.copy-icon { 
					cursor: pointer; 
				} 
				.copy-icon:hover { 
					color: #007bff;
				} 
				.copied-popup { 
					position: absolute;
					user-select: none;
					top: 0;
					left: 0;
					transform: translate(-100%, 0%);
					background-color: #333;
					color: white;
					padding: 5px 10px;
					border-radius: 4px;
					z-index: 100;
					animation: fade-in-out 2s;
				} 
				@keyframes fade-in-out {
					0% {
						opacity: 0;
					}
					4% {
						opacity: 1;
					}
					90% {
						opacity: 1;
					}
					100% {
						opacity: 0
					}
				}
			`} </style>
		</div>
	);
}
```