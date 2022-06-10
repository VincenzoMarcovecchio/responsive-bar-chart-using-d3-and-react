# A responsive bar chart using D3 and React with Typescript
 
 Challenge from frontendmentor.io
 
 
 import React, { useEffect, useRef, useState } from 'react'

import {
  axisBottom,
  axisLeft,
  ScaleBand,
  scaleBand,
  ScaleLinear,
  scaleLinear,
  select,
  selectAll
} from "d3";

import type { IData } from "./types";

interface BarChartProps {
  data: IData[];
}

interface AxisBottomProps {
  scale: ScaleBand<string>;
  transform: string;
}

interface AxisLeftProps {
  scale: ScaleLinear<number, number, never>;
}

interface BarsProps {
  data: BarChartProps["data"];
  height: number;
  scaleX: AxisBottomProps["scale"];
  scaleY: AxisLeftProps["scale"];
  title: any;
}

function AxisBottom({ scale, transform }: AxisBottomProps) {
  const ref = useRef<SVGGElement>(null);

  useEffect(() => {
    if (ref.current) {
      select(ref.current).call(axisBottom(scale));
    }
    select(".domain").remove();
    selectAll(".tick line").attr("y2", 0);
  }, [scale]);

  return <g ref={ref} transform={transform} />
    ;
}

function AxisLeft({ scale }: AxisLeftProps) {
  const ref = useRef<SVGGElement>(null);

  useEffect(() => {
    if (ref.current) {
      select(ref.current).call(axisLeft(scale));
    }
  }, [scale]);

  return;
}

function Bars({ data, height, scaleX, scaleY }: BarsProps) {


  return (
    <>
      {data.map(({ value, label }) => (


        <rect
          key={`bar-${label}`}
          x={scaleX(label)}
          y={scaleY(value)}
          width={scaleX.bandwidth()}
          height={height - scaleY(value)}
          fill="hsl(10, 79%, 65%)"
          rx="2"
          className="bar"
        > <title>${value}</title></rect>
      ))}
    </>
  );
}

export function BarChart({ data }: BarChartProps) {
  const margin = { top: 10, right: 0, bottom: 20, left: 0 };
  const width = 270 - margin.left - margin.right;
  const height = 120 - margin.top - margin.bottom;

  const scaleX = scaleBand()
    .domain(data.map(({ label }) => label))
    .range([0, width])
    .padding(0.3);

  const scaleY = scaleLinear()
    .domain([0, Math.max(...data.map(({ value }) => value))])
    .range([height, 0]);

  const xdim = 270
  const ydim = 120






  return (
    <div className="my_dataviz" id="my_dataviz">
      <svg id="circleBasicTooltip"
        viewBox={`0 0 ${xdim + margin.left + margin.right} ${ydim + margin.top + margin.bottom
          }`}
        preserveAspectRatio="xMinYMin"
        width="100%"
        height="100%"
      >
        <g transform={`translate(${margin.left}, ${margin.top})`}>
          <AxisBottom scale={scaleX} transform={`translate(0, ${height})`} />
          {/* <AxisLeft scale={scaleY} /> */}
          <Bars data={data} height={height} scaleX={scaleX} scaleY={scaleY} />
        </g>
      </svg>
    </div>);
}
                 



 
/////////////////////////////////////////////////////////////////////////////////////////
                 
                 
                 
                 
                 
import React from 'react'
import type { IData } from "../..types";
import { BarChart } from "../../BarChart";


const BAR_CHART_DATA: IData[] = [
  { label: "Mon", value: 100 },
  { label: "Tue", value: 200 },
  { label: "Wed", value: 50 },
  { label: "Thu", value: 150 },
  { label: "Fri", value: 150 },
  { label: "Sat", value: 150 },
  { label: "Sun", value: 150 }
];

 function BarCharta() {

  return (
    <>
      <div style={{ backgroundColor: "hsl(27, 66%, 92%)", position:"absolute", top:"0", zIndex:"999", left:"0",width:"100%",height:"100%",bottom:"0"}}>

        <section className="challenge" style={{ maxWidth: "340px",width:"100%", margin: "auto" }}>
          <div style={{ display: "flex", borderRadius: "5.5px", color: "white", padding: "0.6rem", height: "6rem", margin: "5rem 0 1.3rem 0", backgroundColor: "hsl(10, 79%, 65%)", flexDirection: "row", justifyContent: "space-between", alignItems:"center"}}>
            <div  style={{ display: "flex", flexDirection: "column",  justifyContent: "space-around" }}>
              <div style={{color:"whitesmoke",fontSize:"0.9rem"}}>My balance</div>
              <div style={{marginTop:"0.5rem",fontWeight:"bolder",fontSize:"1.5rem"} }>$ 999.48</div>
            </div>
            <svg width="72" height="48" viewBox="0 0 72 48" xmlns="http://www.w3.org/2000/svg"><g fill="none" fill-rule="evenodd"><circle fill="#382314" cx="48" cy="24" r="24" /><circle stroke="#FFF" stroke-width="2" cx="24" cy="24" r="23" /></g></svg>
          </div>
          <div style={{ display: "flex", borderRadius: "5.5px", backgroundColor: "white", flexDirection: "column" }}>
            <h3 style={{padding:"0.9rem",fontSize:"1.4rem"}}>Spending - Last 7 days</h3>
            <BarChart data={BAR_CHART_DATA} />
            <hr style={{ color: "", width:"calc(100% - 1rem)",margin:"0 auto 1rem auto 0" }} />
         
            <div style={{ display: "flex",padding:"0.9rem", flexDirection: "row", justifyContent: "space-between" }}>

              <div>


                <div  style={{color:"#666",fontSize:"0.9rem"}}>Total this month</div>
                <div style={{color:"hsl(25, 47%, 15%)",fontSize:"2rem",fontWeight:"bolder"}}>$478.48</div>
              </div>
              <div style={{ display: "flex", flexDirection: "column", alignContent:"flex-end",justifyContent: "space-around" }}>
                <div style={{color:"hsl(25, 47%, 15%)", alignSelf:"flex-end" ,fontWeight:"bolder"}}>+24%</div>
                <div style={{color:"#666",fontSize:"0.9rem"}}>From last month</div>
              </div>
            </div> </div>
        </section>
      </div>
    </>
  );

}


export default BarCharta;
