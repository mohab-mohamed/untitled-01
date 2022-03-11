<script>
    import * as d3 from 'd3';
    // import graph from './graph.json';
    import {onMount} from 'svelte';
    import {createEventDispatcher} from 'svelte';

    const graph = {
        nodes: [
            {
                id: '640:477',
                label: '640:477 Intro to Probability',
                group: 'Folder',
                parent: null,
                radius: 2,
                citing_patents_count: 2
            },
            {
                id: '198:111',
                label: '640:477 Intro to Computer Science',
                group: 'Folder',
                parent: null,
                radius: 1,
                citing_patents_count: 1
            },
            {
                id: 'untitled-01',
                label: 'untitled-01',
                group: 'Folder',
                parent: null,
                radius: 1,
                citing_patents_count: 1
            },
            {
                id: 'amazon',
                label: 'amazon',
                group: 'Folder',
                parent: null,
                radius: 1,
                citing_patents_count: 1
            },
            {id: '640:477\\Homework 1', label: 'Homework 1', group: 'File', parent: '640:477'},
            {id: '640:477\\Lecture Notes 1', label: 'Lecture Notes 1', group: 'File', parent: '640:477'},
            {id: '198:111\\Homework 1', label: 'Homework 1', group: 'File', parent: '198:111'},
            {id: 'untitled-01\\design-01', label: 'design-01', group: 'File', parent: 'untitled-01'},
            {id: 'amazon\\principles', label: 'principles', group: 'File', parent: 'amazon'}
        ],
        links: [
            {
                source: '640:477',
                target: '640:477\\Homework 1',
                value: 2
            },
            {
                source: '640:477',
                target: '640:477\\Lecture Notes 1',
                value: 2
            },
            {
                source: '198:111',
                target: '198:111\\Homework 1',
                value: 2
            },
            {
                source: 'untitled-01',
                target: 'untitled-01\\design-01',
                value: 2
            },
            {
                source: 'amazon',
                target: 'amazon\\principles',
                value: 2
            }
        ]
    };
    let chartContainer;

    onMount(() => {
        const chart = ForceGraph(
            graph,
            //@ts-ignore
            {
                nodeId: d => d.id,
                nodeLabel: d => d.label,
                nodeParent: d => d.parent,
                nodeGroup: d => d.group,
                nodeTitle: d => `${d.id} (${d.group})`,
                //@ts-ignore
                width: 1853,
                height: 784,
                //@ts-ignore
                invalidation: null // a promise to stop the simulation when the cell is re-run
            }
        );
        chartContainer.appendChild(chart);
    });

    // Copyright 2021 Observable, Inc.
    // Released under the ISC license.
    // https://observablehq.com/@d3/disjoint-force-directed-graph
    function ForceGraph(
        {
            nodes, // an iterable of node objects (typically [{id}, …])
            links // an iterable of link objects (typically [{source, target}, …])
        },
        {
            nodeId = d => d.id, // given d in nodes, returns a unique identifier (string)
            //@ts-ignore
            nodeGroup, // given d in nodes, returns an (ordinal) value for color
            //@ts-ignore
            nodeGroups, // an array of ordinal values representing the node groups
            //@ts-ignore
            nodeTitle, // given d in nodes, a title string
            nodeLabel, // given d in nodes, a label string
            nodeParent, // given d in nodes, a parent id
            nodeFill = 'currentColor', // node stroke fill (if not using a group color encoding)
            nodeStroke = '#fff', // node stroke color
            nodeStrokeWidth = 1.5, // node stroke width, in pixels
            nodeStrokeOpacity = 1, // node stroke opacity
            nodeRadius = 5, // node radius, in pixels
            //@ts-ignore
            nodeStrength,
            linkSource = ({source}) => source, // given d in links, returns a node identifier string
            linkTarget = ({target}) => target, // given d in links, returns a node identifier string
            linkStroke = '#999', // link stroke color
            linkStrokeOpacity = 0.6, // link stroke opacity
            linkStrokeWidth = 1.5, // given d in links, returns a stroke width in pixels
            linkStrokeLinecap = 'round', // link stroke linecap
            //@ts-ignore
            linkStrength,
            colors = d3.schemeTableau10, // an array of color strings, for the node groups
            width = 640, // outer width, in pixels
            height = 400, // outer height, in pixels
            //@ts-ignore
            invalidation // when this promise resolves, stop the simulation
        } = {}
    ) {
        console.log('links', links);
        // Compute values.
        const N = d3.map(nodes, nodeId).map(intern);
        const LS = d3.map(links, linkSource).map(intern);
        const LT = d3.map(links, linkTarget).map(intern);
        if (nodeTitle === undefined) nodeTitle = (_, i) => N[i];
        const T = nodeTitle == null ? null : d3.map(nodes, nodeTitle);
        const L = nodeLabel == null ? null : d3.map(nodes, nodeLabel); //labels
        const P = nodeParent == null ? null : d3.map(nodes, nodeParent); //parents
        console.log(L);
        const G = nodeGroup == null ? null : d3.map(nodes, nodeGroup).map(intern);
        const W = typeof linkStrokeWidth !== 'function' ? null : d3.map(links, linkStrokeWidth);

        // Replace the input nodes and links with mutable objects for the simulation.
        nodes = d3.map(nodes, (_, i) => ({id: N[i]}));
        links = d3.map(links, (_, i) => ({source: LS[i], target: LT[i]}));

        // Compute default domains.
        if (G && nodeGroups === undefined) nodeGroups = d3.sort(G);

        // Construct the scales.
        const color = nodeGroup == null ? null : d3.scaleOrdinal(nodeGroups, colors);

        // Construct the forces.
        const forceNode = d3.forceManyBody();
        const forceLink = d3.forceLink(links).id(({index: i}) => N[i]);
        if (nodeStrength !== undefined) forceNode.strength(nodeStrength);
        if (linkStrength !== undefined) forceLink.strength(linkStrength);

        const simulation = d3
            .forceSimulation(nodes)
            .force('link', forceLink)
            .force('charge', forceNode)
            .force('x', d3.forceX())
            .force('y', d3.forceY())
            .on('tick', ticked);

        const svg = d3
            .create('svg')
            .attr('width', width)
            .attr('height', height)
            .attr('viewBox', [-width / 2, -height / 2, width, height])
            .attr('style', 'max-width: 100%; height: auto; height: intrinsic;');

        const link = svg
            .append('g')
            .attr('stroke', linkStroke)
            .attr('stroke-opacity', linkStrokeOpacity)
            .attr('stroke-width', typeof linkStrokeWidth !== 'function' ? linkStrokeWidth : null)
            .attr('stroke-linecap', linkStrokeLinecap)
            .selectAll('line')
            .data(links)
            .join('line');

        if (W) link.attr('stroke-width', ({index: i}) => W[i]);

        const node = svg
            .append('g')
            .attr('fill', nodeFill)
            .attr('stroke', nodeStroke)
            .attr('stroke-opacity', nodeStrokeOpacity)
            .attr('stroke-width', nodeStrokeWidth)
            .attr('cursor', 'pointer')
            .selectAll('circle')
            .data(nodes)
            .join('circle')
            .attr('data-label', ({index: i}) => L[i])
            .attr('data-parent', ({index: i}) => P[i])
            .attr('r', nodeRadius)
            .on('click', e => console.log(e.target.dataset))
            .call(drag(simulation));

        console.log(node);

        if (G) node.attr('fill', ({index: i}) => color(G[i]));
        if (T) node.append('title').text(({index: i}) => T[i]);

        // Handle invalidation.
        if (invalidation != null) invalidation.then(() => simulation.stop());

        function intern(value) {
            return value !== null && typeof value === 'object' ? value.valueOf() : value;
        }

        function ticked() {
            link.attr('x1', d => d.source.x)
                .attr('y1', d => d.source.y)
                .attr('x2', d => d.target.x)
                .attr('y2', d => d.target.y);

            node.attr('cx', d => d.x).attr('cy', d => d.y);
        }

        function drag(simulation) {
            function dragstarted(event) {
                if (!event.active) simulation.alphaTarget(0.3).restart();
                event.subject.fx = event.subject.x;
                event.subject.fy = event.subject.y;
            }

            function dragged(event) {
                event.subject.fx = event.x;
                event.subject.fy = event.y;
            }

            function dragended(event) {
                if (!event.active) simulation.alphaTarget(0);
                event.subject.fx = null;
                event.subject.fy = null;
            }

            return d3.drag().on('start', dragstarted).on('drag', dragged).on('end', dragended);
        }

        return Object.assign(svg.node(), {scales: {color}});
    }
</script>

<div bind:this={chartContainer} />
