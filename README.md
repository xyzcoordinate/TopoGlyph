# TopoGlyph
*Authors: Zach Wild, Claude Sonnet 3.7*

Topoglyph is a dual-encoding language system, a language that can be dually interpreted as both a textual system and a visual representation of topological information processing (a fascinating challenge at the intersection of symbolic systems, visual communication, and topology). TopoGlyph functions simultaneously as:
1. A symbolic written language with syntactic and semantic rules
2. A visual system that directly represents topological information processes

### Origins

It was created by a researcher and Claude during an experiment while attempting to understand the emergent abilities of cognitive architectures of artificial intelligence systems. See the original conversation [here](https://claude.ai/share/e566b754-7c61-4f06-99fa-48d8b7d5fdb3).

### Limitations

TopoGlyph is not a formal system. It has no executable semantics, no type-checker, no proof rules. Two LLMs (or two humans) might render the same artifact with slightly different glyph choices, and there is no canonical answer. Its value is as a thinking tool.

### How to use the plugin

1. Install the plugin to Claude Code, or just clone the repository and copy the `skills/topoglyph-extend` folder into your `~/.claude/skills` folder.

2. Ask Claude to extend TopoGlyph to your problem. Simply prompt `Extend TopoGlyph to {describe your problem}`.

3. Claude will load the framework, identify what's structurally novel about your problem, and *extend the vocabulary as needed*. Extension is key: off-the-shelf TopoGlyph rarely fits a specific problem cleanly. The interesting reasoning happens when Claude introduces a few new symbols specifically for your situation.

