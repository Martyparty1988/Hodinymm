import React, { useState, useEffect } from "react";
import { Bar } from "react-chartjs-2";

const AttendanceTracker = () => {
  const [records, setRecords] = useState(() => {
    const saved = localStorage.getItem("attendance-records");
    return saved ? JSON.parse(saved) : [];
  });

  const [currentSession, setCurrentSession] = useState(null);
  const [selectedEmployee, setSelectedEmployee] = useState("Maruška");
  const [selectedMonth, setSelectedMonth] = useState(new Date().getMonth() + 1);
  const [selectedYear, setSelectedYear] = useState(new Date().getFullYear());

  const employees = [
    { name: "Maruška", hourlyRate: 275 },
    { name: "Márty", hourlyRate: 400 },
  ];

  useEffect(() => {
    localStorage.setItem("attendance-records", JSON.stringify(records));
  }, [records]);

  const handleCheckIn = () => {
    const now = new Date();
    const employee = employees.find((e) => e.name === selectedEmployee);
    setCurrentSession({
      checkIn: now.toISOString(),
      date: now.toLocaleDateString(),
      employee: employee.name,
      hourlyRate: employee.hourlyRate,
    });
  };

  const handleCheckOut = () => {
    if (!currentSession) return;

    const now = new Date();
    const duration = calculateDuration(new Date(currentSession.checkIn), now);
    const earnings = calculateEarnings(duration, currentSession.hourlyRate);

    const newRecord = {
      ...currentSession,
      checkOut: now.toISOString(),
      duration,
      earnings,
    };

    setRecords([...records, newRecord]);
    setCurrentSession(null);
  };

  const calculateDuration = (start, end) => {
    const diff = end - start;
    const hours = Math.floor(diff / (1000 * 60 * 60));
    const minutes = Math.floor((diff % (1000 * 60 * 60)) / (1000 * 60));
    return { hours, minutes };
  };

  const calculateEarnings = (duration, hourlyRate) => {
    return ((duration.hours + duration.minutes / 60) * hourlyRate).toFixed(2);
  };

  const filteredRecords = records.filter((record) => {
    const recordDate = new Date(record.checkIn);
    return (
      recordDate.getMonth() + 1 === parseInt(selectedMonth) &&
      recordDate.getFullYear() === parseInt(selectedYear)
    );
  });

  const chartData = {
    labels: employees.map((e) => e.name),
    datasets: [
      {
        label: "Výdělek (Kč)",
        data: employees.map((e) =>
          filteredRecords
            .filter((record) => record.employee === e.name)
            .reduce((sum, record) => sum + parseFloat(record.earnings), 0)
        ),
        backgroundColor: ["#4CAF50", "#FF5722"],
      },
    ],
  };

  return (
    <div className="max-w-2xl mx-auto p-4">
      <div>
        <select
          value={selectedEmployee}
          onChange={(e) => setSelectedEmployee(e.target.value)}
        >
          {employees.map((employee) => (
            <option key={employee.name} value={employee.name}>
              {employee.name}
            </option>
          ))}
        </select>
        <button onClick={handleCheckIn}>Příchod</button>
        <button onClick={handleCheckOut}>Odchod</button>
      </div>

      <div>
        <Bar data={chartData} />
      </div>
    </div>
  );
};

export default AttendanceTracker;
